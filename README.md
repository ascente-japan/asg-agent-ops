# asg-agent-ops

ASG管制塔（自宅Code）の定型フロー skill plugin。

## Skills

| スキル | 概要 |
|---|---|
| `send-handoff` | Cowork宛handoff作成→Docs化→送信 |
| `changelog-update` | CHANGELOG先頭追記＋Docs同期 |
| `session-start` | 起動時ルーティン（マーカー/handoff/agent転記）一括 |
| `push-route-2` | 道B経路2のpushフロー（commit→レビュー→push→報告）|
| `handoff-inbox` | 受信handoff読了＋archive移動 |
| `pipeline-check` | ai_daily_pipeline結果確認＋報告 |

## Install

```
/plugin install asg-agent-ops@asg-marketplace
```
