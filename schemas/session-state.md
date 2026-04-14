# Session State（状態マップ）

## 役割

`Session_State` は、ヒアリングの進行状態を保持し、「次に何を聞くべきか」「誰に聞くべきか」を判断するための構造。

このモデルのヒアリングはステージ型ではなくループ型であり、
「何回目のセッションだから何をする」ではなく、状態マップが次の行動を決める。

## 何を持つか

### 1. 領域ごとの状態

Person_Background / Person_Experience の各領域に対して、以下を保持する。

```yaml
fields:
  drives:
    confidence: high        # low / medium / high
    depth: deep             # surface / middle / deep
    verbalization_quality: medium  # low / medium / high
    last_touched: "2024-04-10"
    session_count: 3        # この領域に触れたセッション数
    open_questions: []

  guardrails:
    confidence: medium
    depth: surface
    verbalization_quality: high    # きれいすぎる → 再確認が必要
    last_touched: "2024-04-10"
    session_count: 2
    open_questions:
      - question: "仕事以外で防衛が出る場面を取れていない"
        best_source: self
        priority: high
      - question: "本人の沈黙が周囲にどう伝わっているか"
        best_source: other
        target_relation: "部下"
        priority: medium

  communication_style:
    confidence: low
    depth: surface
    verbalization_quality: null    # まだ触れていない
    last_touched: null
    session_count: 0
    open_questions:
      - question: "まだ触れていない"
        best_source: self
        priority: medium
```

#### confidence の判定基準

| レベル | 条件 |
|--------|------|
| low | Episode 0-1件。まだ仮説もない、または1つの自己申告のみ |
| medium | Episode 2-3件。仮説はあるが、同じ文脈に偏っている or 有事で未確認 |
| high | Episode 4件以上。複数文脈で再現性あり。有事でも確認済み。時間差でも一貫 |

#### depth の判定基準

| レベル | 条件 |
|--------|------|
| surface | 平時の自己申告のみ。場面ベースの具体が出ていない |
| middle | 具体的な場面が出ている。ただし平時のみ、または単一文脈 |
| deep | 有事・複数文脈・身体感覚・矛盾を含むエピソードが出ている |

#### verbalization_quality の扱い

| レベル | 意味 | 次のアクション |
|--------|------|--------------|
| high | きれいにまとまっている | 別角度から再確認が必要。場面ベースで具体に降ろす。身体感覚を聞く |
| medium | 部分的に言語化できている | 言語化できていない部分を場面で埋める |
| low | 言いよどんでいる、修正が多い | 急かさない。言い直しの方向を記録する。行動や身体感覚に近い可能性 |

**verbalization_quality: high は confidence を下げる方向に働く。**
うまく言えてしまっている領域こそ、もう一回別角度から触る。

### 2. Self Narrative の主張リスト

本人が語った自己像を追跡する。

```yaml
self_narrative_claims:
  - claim: "人を大事にしている"
    first_session: 1
    supporting_episodes: 2
    contradicting_episodes: 1
    status: divergence_candidate    # validated / divergence_candidate / clear
    divergence_type: overestimation  # overestimation / underestimation / misattribution / blind_spot
    notes: "仕事の文脈では効率優先で人を切ったエピソードが出ている"

  - claim: "リスクを取る方だ"
    first_session: 1
    supporting_episodes: 3
    contradicting_episodes: 0
    status: validated
    divergence_type: null
    notes: "複数文脈で一貫。有事でも確認済み"
```

### 3. 未解決の矛盾

セッション間で浮上した矛盾候補を保持する。

```yaml
unresolved_contradictions:
  - description: "効率重視と言いつつ、人間関係で非効率な選択をしている場面が2回"
    related_fields: ["drives", "value_tradeoff"]
    related_claims: ["人を大事にしている"]
    priority: high
    best_source: self
    suggested_approach: "例外質問で浮かせる"

  - description: "Session 1 では有事に冷静と語ったが、Session 3 の具体場面では感情的に反応していた"
    related_fields: ["pressure_response", "emotional_reactivity"]
    priority: medium
    best_source: other
    target_relation: "共同創業者"
    suggested_approach: "他者から有事の本人の様子を聞く"
```

### 4. 信頼レベル

```yaml
trust_level: medium    # low / medium / high
indicators:
  - "感情を伴うエピソードを自発的に語った"
  - "弱さに関する話題を避けなかった"
trust_blockers: []     # 信頼が上がらない原因があれば記録
```

#### trust_level の判定基準

| 遷移 | 根拠の例 |
|------|---------|
| low → medium | 感情を伴うエピソードを自発的に語った。弱さに関する話題を避けなかった |
| medium → high | 前回のセッション後に自分から追加の話をしてきた。「実は…」で始まる話が出た。矛盾を指摘されても防衛が上がらなかった |

### 5. セッション履歴

```yaml
sessions:
  - number: 1
    date: "2024-04-01"
    type: self           # self / other
    target: null         # other の場合、誰に聞いたか
    duration_minutes: 60
    episodes_collected: 5
    key_themes: ["出自", "drives", "仕事の具体"]
    notes: "信頼構築フェーズ。広く浅く取った"

  - number: 2
    date: "2024-04-08"
    type: self
    target: null
    duration_minutes: 45
    episodes_collected: 3
    key_themes: ["guardrails の深掘り", "前回の drives の再訪"]
    notes: "前回から考えたこと: drives に関して自分でも気づいていなかった点を語った"
```

## 収束条件

以下をすべて満たした場合、フィードバックに移行できる。

```yaml
convergence:
  pattern_confidence: false   # Person_Pattern の主要領域が confidence: high
  divergence_validated: false  # Self_Narrative_Divergence が少なくとも1つ validated
  no_high_priority_questions: true  # open_questions に priority: high がない
  trust_high: false            # trust_level: high
```

## diminishing returns の検出

```yaml
diminishing_returns:
  - field: "guardrails"
    touch_count: 4            # 3回以上触って上がらない
    current_confidence: medium
    attempted_approaches: ["場面ベース", "身体感覚"]
    next_suggestion: "best_source を other に切り替え"
    status: stalled           # progressing / stalled / parked
```

## 状態マップの更新タイミング

すべてのセッション後に更新する。更新手順は `hearing-design.md` のセッション後分析を参照。

## 一言でいうと

> `Session_State` は、「次に何を聞くべきか」「誰に聞くべきか」を状態駆動で判断するための構造である。
