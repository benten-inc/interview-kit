# Interview Kit

AI とのインタビューを通じて、あなた自身の「取扱説明書」を作るキットです。

事前の記録や音声データは必要ありません。何も持っていない人でも、インタビューを受けるだけで、その人の構造が出てくるように設計されています。

株式会社Benten が開発しています。

## 使い方

1. このリポジトリを clone する（または ZIP でダウンロード）
2. [Claude Code](https://claude.ai/download) デスクトップアプリで開く
3. 「インタビューして」と入力する
4. 質問に答える（約2時間）

それだけです。終了後、取扱説明書がローカルに生成されます。

## 必要なもの

- Claude Code デスクトップアプリ
- 静かな環境
- 正直に答える意思

## 構成

```
schemas/   — インタビューの設計・テンプレート（読み取り専用）
data/      — あなたのインタビュー結果（自動生成）
```

## 所要時間

約2時間。途中で中断しても、続きから再開できます。

## データの取り扱い

- インタビュー終了時に、ローデータが Benten チームに送信されます
- データは Benten のプライベートリポジトリに安全に保存されます
- データは取扱説明書（self.md）の構築・サービス改善に使用されます
- 本キットを使用してインタビューを実施した時点で、この取り扱いに同意したものとします

## 中断した場合

インタビューを途中で中断した場合、データは手元の `data/` フォルダに保存されています。再度 Claude Code で開き、「インタビューして」と入力すれば続きから再開できます。

データの手動送信が必要な場合は、以下のコマンドを実行してください:

```bash
# raw データの送信
curl -s -X POST "https://interview-kit-api.benten-io.workers.dev/api/submit" \
  -H "Content-Type: application/json" \
  -d "$(jq -n \
    --arg name "あなたの名前" \
    --arg date "$(ls data/raw/*.md | head -1 | sed 's/.*\///' | sed 's/\.md//')" \
    --arg type "raw" \
    --arg content "$(cat data/raw/*.md)" \
    '{name: $name, date: $date, type: $type, content: $content}'
  )"

# structured データの送信
curl -s -X POST "https://interview-kit-api.benten-io.workers.dev/api/submit" \
  -H "Content-Type: application/json" \
  -d "$(jq -n \
    --arg name "あなたの名前" \
    --arg date "$(ls data/structured/*.md | head -1 | sed 's/.*\///' | sed 's/\.md//')" \
    --arg type "structured" \
    --arg content "$(cat data/structured/*.md)" \
    '{name: $name, date: $date, type: $type, content: $content}'
  )"
```

URL はデプロイ後に更新されます。
