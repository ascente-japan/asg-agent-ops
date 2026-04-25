---
name: pipeline-check
description: ai_daily_pipeline.bat の稼働結果を確認し、Cowork宛報告handoff雛形を生成
---

# ai_daily_pipeline 結果確認スキル

22:30稼働の `ai_daily_pipeline.bat`（タスク名: `ASG_AI_daily_pipeline`）の結果確認を担当。
翌朝の実行を想定。

## 確認項目

### 1. Layer 2 稼働ログ生成確認
```bash
ls C:/Users/gamea/scripts/operation_logs/ai_oplog_<YYYYMMDD>.json
```
- 存在すればLayer 2 成功
- 中身が妥当か（各agentのstatus/items_processedが入っているか）1サンプル確認

### 2. Layer 1 HTML日次報告書生成確認
```bash
ls C:/Users/gamea/scripts/daily_reports/ai_daily_<YYYYMMDD>.html
```
- 存在すればLayer 1 成功

### 3. Drive sync確認
```bash
ls "E:/マイドライブ/★仕事用/AI_workspace/daily_reports/ai_daily_<YYYYMMDD>.html"
ls "E:/マイドライブ/★仕事用/AI_workspace/operation_logs/ai_oplog_<YYYYMMDD>.json"
```
両方存在すればDrive sync成功。

### 4. タスクスケジューラ履歴
```powershell
powershell.exe -Command "schtasks /query /tn ASG_AI_daily_pipeline /v /fo LIST | Select-String '前回の結果'"
```
- `0x0` なら成功
- それ以外は失敗コード（`0x1` 等）を確認

## 判定

### 成功ケース（全4項目OK）
Cowork宛に簡潔報告:
```md
## 稼働結果（<YYYYMMDD>）
- Layer 2: ✅ ai_oplog_<YYYYMMDD>.json 生成
- Layer 1: ✅ ai_daily_<YYYYMMDD>.html 生成
- Drive sync: ✅ 両ファイル配置確認
- タスクスケジューラ: ✅ 結果コード 0x0
```

### 失敗ケース（1項目以上NG）
以下を追加収集:
```bash
# タスクスケジューラの実行ログ
powershell.exe -Command "schtasks /query /tn ASG_AI_daily_pipeline /v"
# ai_daily_pipeline.bat の標準出力（タスクスケジューラ経由の場合、stdoutログファイル運用があればそこを参照）
```

Cowork宛にhandoffで詳細報告:
- 失敗ステップ（Layer 2 / Layer 1 / Drive sync のどこ）
- エラー内容（ERRORLEVEL or ファイル非存在）
- 坂井さん操作が必要か、Code側で対処可能かの切り分け

## 実行後アクション

1. 結果handoffを `send-handoff` スキル経由で送信
2. CHANGELOG更新（`changelog-update` スキル）
   - 成功: 1行記録
   - 失敗: 🟡マーカー付きで記録

## 遵守事項
- 成功判定は「件数」ではなく「生成物の存在と構造」で行う（`file-op-verification.md`準拠）
- 失敗時に勝手にretry実行しない（根本原因の切り分けが先）
