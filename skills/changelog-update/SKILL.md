---
name: changelog-update
description: scripts/CHANGELOG.md 先頭にエントリを追加し、GWS MCPでDocs同期する
---

# CHANGELOG更新スキル

本日の作業をCHANGELOGに記録しGoogle Docsへ同期する。

## 実行手順

### 1. エントリ起案
直近の会話・実施作業から以下を凝縮して抽出:
- **完了**: 何をしたか（1行×N）
- **次回TODO**: 次にやるべきこと
- **ブロッカー**: 坂井さんの判断/操作待ち（なければ「なし」）

### 2. `scripts/CHANGELOG.md` 先頭に追記
フォーマット（`.claude/rules/changelog.md`準拠）:
```md
## YYYY/MM/DD【自宅Code】<タイトル>

### 完了
- ...

### 次回TODO
- ...

### ブロッカー
- ...

---
```

配置: 既存の最新エントリ（`## YYYY/MM/DD【...】`）の直前に挿入。

### 3. Docs同期
優先: GWS MCP（`gws docs documents batchUpdate`）で直接書き込み。
フォールバック: `python update_changelog.py`。

Doc ID: `env/.env` の `CHANGELOG_DOC_ID` を参照。

### 4. 🔴🟡マーカー判断
- 🔴至急（坂井さん判断必須・業務影響あり）→ 3重送信（CHANGELOG🔴＋メール＋handoff）
- 🟡重要（要注意・参考）→ CHANGELOGマーカーのみ
- 🟢通常 → マーカーなし

## 遵守事項
- 当月分のみ記載、月初に前月アーカイブ
- 新しい順（上が最新）
- 発信者ラベル `【自宅Code】` 必須
