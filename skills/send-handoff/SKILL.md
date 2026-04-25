---
name: send-handoff
description: Cowork宛handoffを作成・Google Docs化・送付する。引数: 件名（省略時は内容から起案）
---

# handoff送信スキル

Cowork宛handoffの作成→Docs化→送信までを一気通貫で実行する。

## 実行手順

### 1. 件名確定
- 引数で件名が渡されていればそれを使用
- 省略時は直近の作業文脈から `YYYYMMDD_Cowork宛_<件名>` 形式で起案

### 2. 本文作成
- 配置: `C:/Users/gamea/scripts/handoff_tmp/<YYYYMMDD>_Cowork宛_<件名>.md`
- テンプレ:
  ```md
  # 【自宅Code】<件名>

  ## 発信者
  【自宅Code】

  ## 件名
  <1行要約>

  ---

  ## 実施内容 or 確認事項
  <本文>
  ```

### 3. 送信
```bash
cd C:/Users/gamea/scripts && python -X utf8 send_handoff_doc.py "<タイトル>" "C:/Users/gamea/scripts/handoff_tmp/<filename>.md"
```

### 4. URL報告
送信成功後、返却された Docs URL を報告。

## 遵守事項
- `feedback_handoff_use_script.md`: MCP create_file+base64は使わない。必ず`send_handoff_doc.py`経由
- `feedback_cowork_handoff_format.md`: md二重送付禁止（Docsのみ）
- 発信者ラベル `【自宅Code】` 必須
