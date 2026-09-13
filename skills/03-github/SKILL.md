---
name: codex-github
description: Codex 連接 GitHub CLI。說「連接 GitHub」時載入。
---

# 連接 GitHub（Codex 版）

1. 分開執行 `git --version` 與 `gh --version`，相容 Windows PowerShell 5.1。
2. 先執行 `gh auth status`；只有尚未登入時才執行 `gh auth login --web --git-protocol https`。若 Codex 無法讀取 GitHub CLI 設定，取得使用者授權後重跑，不要要求使用者貼 token。
3. 檢查 `git config --global user.name` 與 `git config --global user.email`；缺少時再請使用者提供並設定。
4. 在既有專案先執行 `git status --short --branch`、`git remote -v`、`git branch --show-current`。有 `.git` 但 remote 空白時，不可假設已連上 GitHub。
5. 更新既有 repo：先用 `gh repo list --limit 100` 找到正確 repo，再用 `gh repo clone owner/repo`。clone 後確認 remote；fork 通常是 `origin=自己的 fork`、`upstream=原作者`，修改只推 `origin`。
6. 提交前檢查 diff 與測試；使用 `git add -- 檔名...` 明確加入，不用 `git add .` 把暫存檔一起送出。
7. 提交後使用 `git push origin HEAD`，再確認 branch 與遠端一致。只有使用者明確要求新 repo 時才執行 `gh repo create`。
8. 若只是連線測試才建立測試 repo；成功後詢問保留或刪除。

回報：版本、登入帳號、repo、branch、origin/upstream、commit、push 與驗證結果。不得顯示 token、密碼或一次性驗證碼。
