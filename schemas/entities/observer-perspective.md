# Observer_Perspective

## 役割

`Observer_Perspective` は、ある観測者から見た対象者の像を保持するエンティティ。

これは対象者そのものの真実ではない。  
「その人が他者からどう見えているか」を独立したデータとして残すためにある。

## 何を持つか

主に以下を持つ。

- `observer_person`
- `target_person`
- `relationship_type`
- `relationship_distance`: 近い同僚、頻繁に接する部下、たまに接する他部署など
- `context_scope`: どの文脈を見てそう感じたか
- `perceived_pattern`: どういう人に見えているか
- `perceived_gap`: 何がズレとして感じられているか
- `supportive_view`: 助かったり信頼できると感じる面
- `difficult_view`: きつい、怖い、ズレると感じる面
- `confidence`: その観測者の確信度
- `episode_count`: 何個くらいの場面に基づくか
- `exception_cases`: 普段の印象と違った場面
- `bias_risk`: 関係性や感情による歪みリスク
- `triggering_question`: 状態マップのどの問いに対応してこのヒアリングが走ったか
- `state_map_field`: 状態マップのどの領域に効くか

## 同僚ヒアリングで特に持ちたいもの

同僚から見た像を取る場合は、特に以下を重視する。

- 一緒に働く時に安心する点
- 一緒に働く時に身構える点
- 率直さが助かる場面と、強すぎる場面
- 任せてもらえている感覚と、放置されている感覚
- 黙る / 閉じる / 急ぐ / 詰める ことがどう受け取られているか
- その印象が一回の出来事か、繰り返しのパターンか

## 何に使うか

- 他者ヒアリングを `Inference` に吸収せず残す
- `Person_Pattern` とのズレを比較する
- `Gap_Analysis` を作る材料にする
- 状態マップの `best_source: other` な問いに答える

## いつ取るか

`Observer_Perspective` は独立したフェーズで取るものではない。
状態マップが「この問いは他者に聞いた方が効く」と判断した時に、対象者を特定して取りに行く。
詳細は `hearing-design.md` の「他者セッションの設計」を参照。

## どう作るか

`Observer_Perspective` は、他者から

- どの場面を見てそう思ったか
- その時どう受け取ったか
- その印象は繰り返し出ているか
- 逆にそう見えなかった場面はあるか
- 一緒に働く時に助かる点と難しい点は何か

を聞いた上で作る。

## データ構造のイメージ

```json
{
  "observer_person": "colleague_a",
  "target_person": "founder_x",
  "relationship_type": "colleague",
  "relationship_distance": "works_closely",
  "context_scope": [
    "1on1",
    "weekly_meeting",
    "urgent_issue"
  ],
  "perceived_pattern": [
    "率直で判断が速い",
    "考えている時は急に閉じる",
    "任せる時の基準共有が少ない"
  ],
  "perceived_gap": [
    "率直さが威圧に見えることがある",
    "任せるが放置に見えることがある"
  ],
  "supportive_view": [
    "意思決定が速い",
    "重要な論点を外さない"
  ],
  "difficult_view": [
    "急に反応がなくなると拒絶に感じる",
    "基準が伝わらないまま任されると不安"
  ],
  "episode_count": 5,
  "exception_cases": [
    "少人数の1on1ではかなり柔らかい"
  ],
  "confidence": "medium",
  "bias_risk": "observer_is_currently_exhausted"
}
```

## やらないこと

- 観測者のラベル評価だけをそのまま真実扱いしない
- どの場面を見てそう思ったかを落とさない
- 関係バイアスを無視しない

## 一言でいうと

> `Observer_Perspective` は、「Aさんから見たBさんの像」を独立して保持するためのエンティティである。
