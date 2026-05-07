# Episodes

## 役割

`Episodes` は、このモデルにおける最小の観測単位。

何が起きたか、誰と起きたか、その時どう振る舞ったか、どう感じたか、結果どうなったかを保持する。

このモデルでは、まず `Episodes` を集め、その後に `Inferences` を作る。  
つまり `Episodes` は、解釈の出発点であり、証拠の原本である。

## 最低限持ちたい項目

- `what_happened`
- `who_was_there`
- `context`: 場の属性オブジェクト（下記）
- `observed_action`
- `felt_state`
- `outcome`
- `recovery`
- `self_interpretation`

### context オブジェクト

旧設計では `power_gradient` / `public_private` / `normal_or_pressure` を独立フィールドとして持っていたが、3つとも「場の属性」として一緒に扱うことが多いため、1オブジェクトに統合する。

```yaml
context:
  power: symmetric | subject_higher | subject_lower  # 権力差
  setting: public | private                           # 公/私
  pressure: normal | pressure                         # 平時/有事
```

必要に応じて部分埋めでよい（該当しないフィールドは省略）。

## セッション追跡フィールド

ループ型ヒアリングにおいて、Episode がいつ・どの文脈で取れたかを追跡する。

- `session_date`: いつのセッションで取れたか
- `session_number`: 通算何回目のセッションか
- `is_revisit`: 過去に触れたテーマの再訪か
- `revisit_of`: どのエピソードの再訪か（再訪の場合のみ）

`is_revisit: true` の Episode は、前回との突合による Inference 生成の材料になる。

## Session への参照フィールド

Session を first-class entity として扱うため、Episode は元となる session を参照する。

- `session_ref`: 元 session の識別子（本キットでは `data/raw/{YYYY-MM-DD}.md` の日付）
- `timestamp`: session 内の該当箇所（Q番号）
- `speaker`: 発話者の person_id（**推定で可**。participants 内の誰かであることは確定しているため精度は重要ではない）
- `about`: この Episode が誰のモデルに反映されるか（**厳密に**）
  - 各エントリは `id` / `confidence` (high | medium | low) / `basis`（根拠）を持つ
  - `confidence: high` のみ自動で反映、`medium` は要検証フラグ付きで反映、`low` は保留プールへ
- `mentioned`: 本人以外で言及された人物のリスト
  - `name` を持つ（必要に応じて識別子を付与）

### なぜ speaker と about が非対称か

- **speaker を間違えても** 影響は軽微。participants の中の誰かであることは確定している
- **about を間違えると** 人物像が壊れる。誤帰属は downstream の成果物まで cascade する

したがって speaker は推定、about は confidence 付きで慎重にタグ付けする。

### 例

```yaml
- session_ref: "2026-04-14"
  timestamp: "Q12"
  speaker: self
  about:
    - id: self
      confidence: high
      basis: "自分自身の特性を語っている"
  mentioned:
    - name: 松崎
```

## アプリケーションによって追加される項目の例

- `execution_pattern`
- `contextual_shift`
- `pressure_response`
- `authority_expression`
- `delegation_gap`
- `presence_cost`
- `symbolic_load`

## Episode の3種（別エンティティではなく、speaker/about の組み合わせで区別）

旧設計では Observer_Perspective / Relational_Episodes を別エンティティとして持っていたが、
speaker と about があれば同じ Episode スキーマで統一的に扱える。

| 種別 | speaker | about | 旧エンティティ |
|------|---------|-------|---------------|
| 本人の自己開示 Episode | 本人 | 本人 | Episode |
| 他者視点の Episode | 他者 | 本人 | Observer_Perspective |
| 関係的 Episode | either | [本人, 他者] | Relational_Episodes |

他者視点の Episode から生まれる解釈は、通常通り `Inferences` に入る。
`Inference.source: self_reported | observer` を付ければ、どの視点由来の仮説かを追跡できる。

## 何に使うか

- `Person_Background` と `Person_Experience` の仮説を推定する
- `Inferences` を生成する
- `Validation` の証拠にする
- `Person_Pattern` を更新する根拠にする
- `Cognitive_Divergence`（self_vs_other / narrative_vs_action）の材料にする

## 良い Episode の条件

- 具体的である
- 時系列が分かる
- 相手と場が分かる
- 感情だけでなく行動が入っている
- 結果が分かる
- 後から見た自己解釈が分かる

## やらないこと

- `Episode` の中に解釈を混ぜすぎない
- 単発の Episode をそのまま本質扱いしない
- 都合の良い場面だけ集めない

## 一言でいうと

> `Episodes` は、人を決めつける前に、まず何が起きたかを残すための観測単位である。
