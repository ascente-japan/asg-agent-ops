---
name: session-start
description: セッション開始時ルーティン（CHANGELOG確認・handoff新着チェック・agent CHANGELOG転記）を一括実行
---

# セッション開始ルーティン

毎セッション開始時に以下を実行する。

## 実行手順

### 1. CHANGELOG 先頭マーカー確認
- Doc ID: `env/.env` の `CHANGELOG_DOC_ID` 参照
- 先頭に 🔴（至急）/ 🟡（要注意）マーカーがあれば坂井さんに即共有
- なければ現状報告不要

### 2. 会社Cowork → 自宅Code handoff新着チェック
- 対象ディレクトリ: `E:/その他のパソコン/マイ ノートパソコン/Cowork運用設計/handoff/`
- 命名: `YYYYMMDD_Code宛_*.md` の直下ファイルが未処理
- 新着があれば一覧表示。中身は必要に応じてRead
- 読了後は該当ファイルを `archive/YYYYMM/` に移動（`handoff-inbox`スキル参照）

### 3. 各agent CHANGELOG.md 転記チェック
対象agent（registry.md準拠）:
- `C:/Users/gamea/kuchikomi365-agent/CHANGELOG.md`
- `C:/Users/gamea/rakuten-agent/CHANGELOG.md`
- `C:/Users/gamea/cybozu-agent/CHANGELOG.md`

各agentのCHANGELOG.md先頭エントリと、統合Docsに転記済みの最新日付を突合。未転記があれば凝縮して転記（`feedback_changelog_transfer.md`準拠）。

### 4. 報告
以下を坂井さんに1発で返す:
```
CHANGELOGマーカー: 🔴なし / 🟡なし（または検出内容）
handoff新着: N件（ファイル名）
agent転記: 完了 / 未対応N件
```

## 遵守事項
- 🔴3重送信（CHANGELOG+メール+handoff）は秘書AIではなく坂井さん宛
- handoff新着は読了と同時にarchive移動（直下残存=未処理の原則）
