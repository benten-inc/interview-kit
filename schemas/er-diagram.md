# ER Diagram / Blueprint

このファイルでは、Benten の schema を2つの観点で示す。

- エンティティがどう繋がるか
- 実際にどう作られていくか

---

## 1. Entity Relation

```mermaid
flowchart LR
    PERSON[PERSON]

    BACKGROUND[PERSON_BACKGROUND<br/>OS仮説<br/>extroversion_introversion<br/>abstract_concrete<br/>risk_appetite<br/>emotional_reactivity]
    EXPERIENCE[PERSON_EXPERIENCE<br/>形成済み構造仮説<br/>drives<br/>self_narrative<br/>guardrails]

    EPISODES[EPISODES<br/>本人に関する観測事実<br/>基本フォーマット<br/>+ 認知ギャップ構造（4次元）<br/>+ 対比構造<br/>+ 意思決定の軸<br/>+ 事業選択の構造]

    REL_EPISODES[RELATIONAL_EPISODES<br/>他者との間で起きた事実<br/>relationship_type<br/>who_said_what<br/>observer_reaction<br/>outcome]

    OBSERVER_VIEW[OBSERVER_PERSPECTIVE<br/>他者から見た本人像<br/>perceived_pattern<br/>perceived_gap<br/>confidence<br/>bias_risk]

    INFERENCES[INFERENCES<br/>Episodeから生まれる仮説<br/>例: 評価恐怖 / 権力差反応 / 威圧化]

    VALIDATION[VALIDATION<br/>矛盾チェック<br/>反証確認<br/>一貫性確認<br/>採択 / 保留 / 棄却]

    PATTERN[PERSON_PATTERN<br/>検証を通過した本人構造<br/>OS / drives / guardrails<br/>execution_pattern<br/>contextual_shift<br/>pressure_response<br/>best_environment]

    GAP[GAP_ANALYSIS<br/>認知ギャップ（4次元）<br/>価値観レイヤーのズレ<br/>抽象度のズレ<br/>前提の深さのズレ<br/>視点の高さのズレ]

    PERSON --> BACKGROUND
    PERSON --> EXPERIENCE
    PERSON --> EPISODES

    EPISODES -->|本人に関する観測| INFERENCES
    REL_EPISODES -->|他者との間で起きた出来事| OBSERVER_VIEW

    BACKGROUND -.->|影響要因として参照| INFERENCES
    EXPERIENCE -.->|形成済み構造として参照| INFERENCES
    PATTERN -.->|既存仮説として参照| INFERENCES
    OBSERVER_VIEW -.->|他者視点として参照| INFERENCES

    INFERENCES --> VALIDATION
    VALIDATION --> PATTERN

    PATTERN --> GAP
    OBSERVER_VIEW --> GAP
    REL_EPISODES --> GAP
    VALIDATION --> GAP
```

---

## 2. Blueprint

このモデルは、最初から `Person_Pattern` を決めるのではない。  
まず基盤ヒアリングを行い、LLM で `Background` と `Experience` の仮説を集約し、その後アプリケーション文脈の Episode を蓄積していく。

```mermaid
flowchart TD
    A1[初回ヒアリング<br/>基盤ヒアリングを行う] --> A2[基盤エピソードを収集<br/>人と会った後どうなるか<br/>否定された時どう反応するか<br/>何を報酬と感じるか]
    A2 --> A3[LLMで集約<br/>基盤エピソードを仮説化]

    A3 --> B1[PERSON_BACKGROUND を更新<br/>extroversion_introversion<br/>abstract_concrete<br/>risk_appetite<br/>emotional_reactivity]
    A3 --> B2[PERSON_EXPERIENCE を更新<br/>drives<br/>self_narrative<br/>guardrails]

    B1 --> C1[アプリケーション層ヒアリング開始]
    B2 --> C1

    C1 --> C2[アプリ層の EPISODES を収集<br/>経営者の認知ギャップが出そうな場面<br/>会食 / 採用 / 商談 / 1on1 / 有事]

    C2 --> C3[必要に応じて他者ヒアリング<br/>部下 / 共同創業者 / 投資家 / 顧客]
    C3 --> C4[RELATIONAL_EPISODES を作成]
    C4 --> C5[OBSERVER_PERSPECTIVE を作成]

    C2 --> D1[LLMで EPISODES を仮説化]
    C5 --> D1
    B1 -.参照.-> D1
    B2 -.参照.-> D1

    D1 --> D2[INFERENCES を生成]
    D2 --> D3[VALIDATION<br/>複数文脈での一貫性<br/>反証の有無<br/>自己像と他者像のズレ]

    D3 --> E1[PERSON_PATTERN を更新]
    D3 --> E2[GAP_ANALYSIS を更新<br/>4次元で構造化]

    E1 --> F1[この人はどういう構造か]
    E2 --> F2[どの次元でズレが起きやすいか<br/>価値観/抽象度/前提/視点の高さ]
```

---

## 3. 実際のイメージ

### フェーズ1

- 本人への基盤ヒアリングを行う
- 具体的なエピソードを集める
- LLM で `Person_Background` と `Person_Experience` の仮説に整理する

### フェーズ2

- アプリケーション文脈で Episode を継続収集する
- 経営者なら、率直さ、委任、閉じる時間、会食後の消耗などが出る場面を取る

### フェーズ3

- 他者ヒアリングで `Relational_Episodes` と `Observer_Perspective` を作る
- 本人の意図と、相手の受け取りのズレを残す

### フェーズ4

- LLM が `Inferences` を生成する
- `Validation` で採択 / 保留 / 棄却する
- `Person_Pattern` と `Gap_Analysis` を更新する

---

## 4. SESSION によるプロヴェナンス追跡（案）

更新内容ごとに「誰が喋ったか」を持つとノイズが入る。
代わりに「どの会話セッションに由来するか」だけを追跡する。
SESSION を見れば参加者は分かるので、必要な時に原文まで辿れる。

### 変更点

| 変更 | 内容 |
|------|------|
| **追加** | `SESSION` エンティティ。会話1回が1レコード。参加者・日時・文脈を持つ |
| **追加** | 各エンティティに `source_session_id`。更新内容がどの会話に由来するかを追跡 |
| **不要** | 個別データポイントごとの「話者」カラム。誰がいたかは SESSION の `participant_ids` で引ける |

### ER（SESSION 追加版）

```mermaid
erDiagram
    PERSON {
        string id
        string name
    }

    SESSION {
        string id
        date date
        string title "例: 南さん@大宮"
        string[] participant_ids "参加者のPERSON ID"
        int duration_min
        string context "自然会話 / 構造化インタビュー / 他者ヒアリング等"
        string raw_file_path "raw/ へのパス"
    }

    PERSON_BACKGROUND {
        string person_id
        string dimension "abstract_concrete等"
        string content
        string source_session_id
    }

    PERSON_EXPERIENCE {
        string person_id
        string layer "drives / guardrails / self_narrative"
        string content
        string source_session_id
    }

    EPISODES {
        string id
        string person_id "この人に関するエピソード"
        string source_session_id
        string what_happened
        string observed_action
        string self_interpretation
    }

    RELATIONAL_EPISODES {
        string id
        string observer_person_id
        string target_person_id
        string source_session_id
        string what_happened
        string who_said_what
    }

    OBSERVER_PERSPECTIVE {
        string id
        string observer_person_id "見ている人"
        string target_person_id "見られている人"
        string source_session_id
        string perceived_pattern
        string perceived_gap
    }

    INFERENCES {
        string id
        string person_id
        string source_episode_id
        string content
    }

    VALIDATION {
        string id
        string inference_id
        string result "adopted / held / rejected"
    }

    PERSON_PATTERN {
        string person_id
        string content
    }

    GAP_ANALYSIS {
        string person_id
        string gap_type
        string content
    }

    PERSON ||--o{ SESSION : "participates_in"
    SESSION ||--o{ EPISODES : "produces"
    SESSION ||--o{ RELATIONAL_EPISODES : "produces"
    SESSION ||--o{ OBSERVER_PERSPECTIVE : "produces"
    SESSION ||--o{ PERSON_BACKGROUND : "updates"
    SESSION ||--o{ PERSON_EXPERIENCE : "updates"

    PERSON ||--|| PERSON_BACKGROUND : "has"
    PERSON ||--|| PERSON_EXPERIENCE : "has"
    PERSON ||--o{ EPISODES : "about"

    EPISODES ||--o{ INFERENCES : "generates"
    RELATIONAL_EPISODES ||--o{ OBSERVER_PERSPECTIVE : "grounds"

    INFERENCES ||--o{ VALIDATION : "checked_by"
    VALIDATION ||--o{ PERSON_PATTERN : "updates"
    VALIDATION ||--o{ GAP_ANALYSIS : "updates"

    PERSON ||--|| PERSON_PATTERN : "has"
    PERSON ||--|| GAP_ANALYSIS : "has"
    OBSERVER_PERSPECTIVE }o--|| GAP_ANALYSIS : "feeds"
```

### ポイント

- 「誰が喋ったか」ではなく「どの会話から来たか」だけを持つ。SESSION を見れば参加者は分かる
- 同一人物に対する OBSERVER_PERSPECTIVE が複数 SESSION から蓄積される場合、`source_session_id` で時系列を追える
- `raw_file_path` を SESSION が持つので、詳細を掘りたい時は原文に戻れる

---

## 一言でいうと

> 最初のヒアリングで `Background` と `Experience` の仮説を作り、  
> その後アプリケーション文脈の Episode と他者視点を足しながら、  
> `Person_Pattern` と `Gap_Analysis` を更新していくのがこの blueprint である。  
> 全ての更新は `SESSION` を経由し、由来する会話まで追跡可能にする。
