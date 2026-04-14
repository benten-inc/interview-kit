# Inferences

## 役割

`Inferences` は、観測から生まれる意味づけや仮説を保持するエンティティ。

`Episode` は観測事実であり、`Inference` はその観測に対して与えられた読みである。
この2つを分けることが、このモデルの中核になる。

## 何を持つか

`Inferences` は2つの層を持つ。

### Episode 単位の Inference

1つの Episode から生まれる読み。

- 評価恐怖が強く出た可能性がある
- 権力差でモードが切り替わった可能性がある
- 主体性ではなく欠乏駆動だった可能性がある
- Defensive Mode が出ており、責任回避が優先された可能性がある
- 率直さが威圧として伝わった可能性がある

### セッション横断の Inference

複数セッションの突合から生まれる読み。時間差を使った検証の核になる。

- Session 1 では「人を大事にしている」と語ったが、Session 3 では効率優先で人を切ったエピソードが出た。overestimation の可能性がある
- Session 2 と Session 4 で同じ構造が別文脈から出てきた。再現性が高い
- 前回きれいに言語化されていた drives が、有事エピソードでは別の形で現れた。言語化と実際の駆動力にズレがある可能性がある
- 本人が使った言葉と、実際の行動の間に温度差がある。言語化の精度に問題がある可能性

セッション横断の Inference は、状態マップの `unresolved_contradictions` や `self_narrative_claims` の更新材料になる。

## 何に使うか

- `Episode` を構造仮説に変換する
- 複数 Episode を横断して `Validation` する材料にする
- `Person_Pattern` の更新候補を作る

## 作り方

`Inference` を作る時は、

- `Episode` の事実
- `Relational_Episodes` の事実
- `Person_Background` の仮説
- `Person_Experience` の仮説
- 既存の `Person_Pattern`
- `Observer_Perspective`

を参照してよい。  
ただし、どれを参照しても `Inference` 自体は仮説であり、真実として固定してはならない。

## やらないこと

- 1つの `Inference` を真実扱いしない
- `Episode` の記述と `Inference` の記述を混ぜない
- もっともらしい説明を優先して反証を飛ばさない

## 一言でいうと

> `Inferences` は、観測事実から生まれる「読み」を仮説として保持するためのエンティティである。
