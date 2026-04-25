---
name: handoff-inbox
description: 会社Coworkからの未読handoff一覧表示＋指定ファイル読了後のarchive移動
---

# handoff受信箱スキル

会社Cowork → 自宅Code の handoff受信・読了後の後処理を担当。

## 受信ディレクトリ

```
E:/その他のパソコン/マイ ノートパソコン/Cowork運用設計/handoff/
```

命名: `YYYYMMDD_Code宛_<件名>.md`

## 実行手順

### 1. 未読一覧表示
直下の `YYYYMMDD_Code宛_*.md` ファイルを列挙（archive/配下は除外）。
新しい日付順に表示。

### 2. 読了
指定ファイル（または全件）をReadで読み込む。
内容に対応アクションがあれば、別スキル（send-handoff, push-route-2, changelog-update等）に連鎖。

### 3. archive移動
読了したファイルは `handoff/archive/YYYYMM/` に移動。
```bash
# 例: 2026-04のファイル
mv "handoff/20260424_Code宛_xxx.md" "handoff/archive/202604/"
```

ディレクトリ不在時は `mkdir -p archive/YYYYMM` で先に作成。

### 4. 報告
```
読了: <ファイル名>
archive移動: ✅
次アクション: <ある場合のみ>
```

## 遵守事項
- handoff直下残存 = 未処理のサイン。読了と同時に移動
- 読むだけで対応不要なhandoffでもarchiveに移動する（処理ログとして機能）
- 移動前に中身のReadは必須（スキップで移動したら見落としリスク）
