# Relational_Episodes

## 役割

`Relational_Episodes` は、二者関係の中で何が起きたかを保持するエンティティ。

通常の `Episodes` が「本人に起きた出来事」を中心に持つのに対して、  
`Relational_Episodes` は「本人と他者の間で、どんなやり取りやズレが起きたか」を持つ。

特に、経営者の認知ギャップを扱う時には重要になる。

## 何を持つか

最低限、以下を持ちたい。

- `target_person`: 対象者は誰か
- `observer_person`: 相手は誰か
- `relationship_type`: 部下、共同創業者、投資家、顧客、候補者など
- `relationship_distance`: 日常的に近いか、断続的か
- `what_happened`: 二者の間で何が起きたか
- `power_gradient`: 権力差はどうだったか
- `public_private`: 公の場か、私的な場か
- `normal_or_pressure`: 平時か、緊急時か
- `who_said_what`: 誰が何を言ったか
- `observer_reaction`: 相手はどう受け取ったか
- `observer_feeling`: 相手はどう感じたか
- `target_intent_if_known`: 対象者の意図が分かる場合は何だったか
- `outcome`: 関係や仕事にどんな結果が出たか

## 何に使うか

- 他者ヒアリングの原本にする
- `Observer_Perspective` を作る材料にする
- `Gap_Analysis` の証拠にする

## 良い Relational Episode の条件

- 具体的な場面がある
- 二者のやり取りが分かる
- 受け取りの差が見える
- 権力差と場の条件が分かる
- 仕事や関係への結果が分かる

## やらないこと

- 相手の印象だけで終わらせない
- 二者の関係文脈を落とさない
- 一回のすれ違いで構造を固定しない

## 一言でいうと

> `Relational_Episodes` は、本人と他者の間でどんなズレが起きたかを、場面単位で保持するためのエンティティである。
