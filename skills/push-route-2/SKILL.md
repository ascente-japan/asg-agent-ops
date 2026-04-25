---
name: push-route-2
description: 道B経路2のpushフロー（commit→レビュー依頼handoff→OK後push→完了報告）を実行
---

# 道B経路2 pushフロー

非定型push（コード変更・README・rules/等）の道B経路2準拠フロー。
rules/github.md 厳守。

## 前提判定

```
変更は自動ツールが作ったもの？
 ├─ Yes → 経路1（単独push、このスキルは不要）
 └─ No  → 経路2（このスキル）
```

## フェーズ1: レビュー依頼（このスキル発火時）

### 1. 現状確認
```bash
git status
git log -1 --stat
git diff HEAD~1  # 直近commitの差分
```

### 2. commit未実施なら commit
- **`git add -A` 禁止**（明示パスのみ）
- commitメッセージ規約: Conventional Commits風（`feat:`, `fix:`, `chore:` 等）

### 3. レビュー依頼handoff生成
配置: `handoff_tmp/<YYYYMMDD>_Cowork宛_<件名>push経路2レビュー依頼.md`

テンプレ:
```md
# 【自宅Code】push経路2 レビュー依頼（<件名>）

## commit
- hash: <short hash>
- message: <full message>
- <N files changed, +X, -Y>

## 変更要旨
<箇条書き>

## diff（要約）
```diff
<重要箇所のみ>
```

## 確認事項
1. <point>
2. <push順（複数repoの場合）>
```

### 4. handoff送信
`send-handoff`スキル経由 or 直接 `send_handoff_doc.py`。

### 5. 待機
Coworkレビュー結果handoffを待つ（坂井さん経由）。

## フェーズ2: OK受領後のpush

### 1. push実行
```bash
git push origin main
```

worktreeの場合、non-fast-forwardなら `git rebase origin/main` → push。

### 2. 完了報告handoff生成・送信
配置: `handoff_tmp/<YYYYMMDD>_Cowork宛_<件名>push完了報告.md`

テンプレ:
```md
# 【自宅Code】push完了報告（<件名>）

## 実施結果
| リポ | commit | push結果 |
|---|---|---|
| ... | <hash> | ✅ <before>..<after> main→main |

## rebase有無
（あれば元hashと新hashを明記）

## 道B経路2 カウント
N/5 回目 無事故
```

### 3. CHANGELOG更新
`changelog-update`スキル発火（任意、push内容が重要なら）。

## 遵守事項
- **push禁止のままhandoff提出**（フェーズ1でOK前にpushしない）
- push順明示指示がなければリポ名昇順（cybozu → kuchikomi365 → rakuten → scripts）
- force push禁止
- hooks skip（--no-verify）禁止
- 失効条件: 事故ゼロ3〜5回で段階的省略（rules/github.md §道B降格カウント）
