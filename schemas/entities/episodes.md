# Episodes

## 役割

`Episodes` は、このモデルにおける最小の観測単位。

何が起きたか、誰と起きたか、その時どう振る舞ったか、どう感じたか、結果どうなったかを保持する。

このモデルでは、まず `Episodes` を集め、その後に `Inferences` を作る。  
つまり `Episodes` は、解釈の出発点であり、証拠の原本である。

## 最低限持ちたい項目

- `what_happened`
- `who_was_there`
- `power_gradient`
- `public_private`
- `normal_or_pressure`
- `observed_action`
- `felt_state`
- `outcome`
- `recovery`
- `self_interpretation`

## アプリケーションによって追加される項目の例

- `execution_pattern`
- `contextual_shift`
- `pressure_response`
- `authority_expression`
- `delegation_gap`
- `presence_cost`
- `symbolic_load`

## 何に使うか

- `Person_Background` と `Person_Experience` の仮説を推定する
- `Inferences` を生成する
- `Validation` の証拠にする
- `Person_Pattern` を更新する根拠にする

二者関係の中で起きたズレを読む場合は、`Episodes` だけでなく `Relational_Episodes` も併用する。

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
