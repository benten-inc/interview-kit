# Gap_Analysis

## 役割

`Gap_Analysis` は、`Person_Pattern`、`Observer_Perspective`、`Relational_Episodes` のズレを統合し、  
最終的に「どこに認知ギャップがあるか」を保持するエンティティ。

## 何を持つか

主に以下を持つ。

- `target_person`
- `gap_type`
- `gap_summary`
- `self_side`: 本人側ではどうだったか
- `other_side`: 他者側ではどう見えたか
- `context_scope`: どの文脈で起きるか
- `evidence_relational_episodes`
- `evidence_observer_perspectives`
- `confidence`
- `repair_hint`

## gap_type の考え方

`gap_type` は、「本人の意図や構造が、他者には何として受け取られやすいか」を表す。

このモデルでは、`gap_type` を再利用可能な型として持つ。  
そうすることで、同じ人の中で繰り返し起きるズレも、複数の経営者に共通して起きるズレも比較しやすくなる。

## 主な gap_type

### 強さが別の意味で読まれる型

- `directness_vs_threat`
- `high_standard_vs_coldness`
- `speed_vs_sloppiness`
- `crisis_control_vs_dominance`

意味:

- 率直さが威圧に見える
- 高い基準が冷たさに見える
- 速さが雑さに見える
- 有事対応の強さが支配性に見える

### 距離の取り方が別の意味で読まれる型

- `withdrawal_vs_rejection`
- `silence_vs_disapproval`
- `energy_management_vs_favoritism`

意味:

- 閉じることが拒絶に見える
- 沈黙が否定に見える
- 距離調整が好き嫌いに見える

### 任せ方が別の意味で読まれる型

- `delegation_vs_abandonment`
- `support_vs_micromanagement`
- `ownership_vs_distrust`

意味:

- 任せることが放置に見える
- 支援が介入しすぎに見える
- 自分で持つことが信頼していないように見える

### 思考の仕方が別の意味で読まれる型

- `structure_vs_lack_of_empathy`
- `optimism_vs_denial`
- `risk_sensitivity_vs_brake`
- `focus_vs_discard`

意味:

- 構造化が非共感に見える
- 楽観が現実逃避に見える
- リスク感度がブレーキに見える
- 選択と集中が切り捨てに見える

### 出力と内側のコストがズレる型

- `visible_strength_vs_hidden_cost`
- `social_skill_vs_natural_sociability`
- `consistency_shift_vs_instability`

意味:

- 周囲には強く見えるが、本人はかなり消耗している
- 社交 skill が自然な外向性に見える
- 学習による変化が一貫性のなさに見える

## よく使う gap_type

実務上まず持っておくなら、以下が優先度高い。

- `directness_vs_threat`
- `high_standard_vs_coldness`
- `speed_vs_sloppiness`
- `delegation_vs_abandonment`
- `withdrawal_vs_rejection`
- `silence_vs_disapproval`
- `structure_vs_lack_of_empathy`
- `crisis_control_vs_dominance`
- `visible_strength_vs_hidden_cost`
- `social_skill_vs_natural_sociability`

## gap_type の付け方

1つの Episode で `gap_type` を確定しない。  
少なくとも以下を見てから付ける。

- 本人の `Person_Pattern`
- 他者の `Observer_Perspective`
- 複数の `Relational_Episodes`
- `Validation` の結果

同じズレが複数文脈で出ているなら、強い `gap_type` として保持できる。

## 自己認知ギャップ（Self_Narrative_Divergence）

Gap_Analysis は **本人 vs 他者** のズレを扱うが、
Self_Narrative_Divergence は **本人が言ってること vs 本人がやってること** のズレを扱う。

### divergence_type

| 類型 | 意味 | 例 |
|------|------|-----|
| `overestimation` | 自分はできていると思っているが、Episodes が裏付けない | 「人を大事にしてる」→ 効率優先で人を切っている |
| `underestimation` | 自分にはないと思っているが、他者や Episodes から見えている | 「リーダーシップがない」→ 周囲は頼りにしている |
| `misattribution` | 行動の理由が本人の認識と違う | 「論理で決めてる」→ 実は感情で決めている |
| `blind_spot` | そもそもその領域を認識していない | 自分の沈黙が周囲に与える影響に気づいていない |

### フィールド

- `narrative_claim`: 本人が語っている自己像
- `divergence_type`: overestimation / underestimation / misattribution / blind_spot
- `evidence_episodes`: 乖離を示す Episodes のリスト
- `evidence_observer`: Observer_Perspective からの補強材料
- `recurrence`: 複数文脈で再現しているか
- `confidence`: 確度
- `awareness`: 本人がこの乖離に気づいているか
- `feedback_status`: 未伝達 / 伝達済み / 受容 / 拒否 / 保留

### gap_type との関係

- `gap_type` は対人のズレ（他者にどう読まれるか）
- `divergence_type` は自己認知のズレ（自分で自分をどう読み違えているか）
- 両方が重なる場合がある（例: 「任せてる」と思っている + 周囲には放置に見えている + 実際の Episodes でもフォローしていない → overestimation + delegation_vs_abandonment）

## 何に使うか

- 本人の意図と相手の受け取りのズレを要約する
- 本人の自己像と実際の行動のズレを要約する
- 経営者の認知ギャップを最終的に表現する
- 「お前違ってるよ」と言うための根拠を保持する
- 改善や修復の起点をつくる

## repair_hint の考え方

`repair_hint` は、gap を埋めるための仮説を持つ。

たとえば:

- `directness_vs_threat`
  - 先に意図を言う
  - 強い指摘の前に評価軸を共有する
- `delegation_vs_abandonment`
  - 任せる時に基準と確認頻度を明示する
- `withdrawal_vs_rejection`
  - 閉じる前に「考える時間を取る」と宣言する
- `visible_strength_vs_hidden_cost`
  - 回復時間を明示し、周囲の誤読を防ぐ

## どう作るか

`Gap_Analysis` は、以下を合わせて作る。

- `Person_Pattern`
- `Observer_Perspective`
- `Relational_Episodes`
- `Validation`

つまり、単なる印象の寄せ集めではなく、検証を通過したズレだけを残す。

## やらないこと

- ラベルだけを並べて終わらせない
- 本人か他者のどちらかだけを正しいと決めない
- 証拠が薄いまま強い gap を作らない

## 一言でいうと

> `Gap_Analysis` は、その人がどこでどう誤読されやすいかを、検証済みのズレとして保持するためのエンティティである。
