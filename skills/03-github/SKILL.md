---
name: codex-github
description: Codex 連接及安全操作 GitHub CLI。說「連接 GitHub」「更新回 GitHub」「push 專案」時載入。
---

# 連接 GitHub（Codex 版）

1. 檢查 `git --version`、`gh --version`、`gh auth status`；未登入才執行 `gh auth login --web --git-protocol https`。
2. 更新既有專案前先執行 `git rev-parse --show-toplevel`、`git status --short`、`git branch --show-current`、`git remote -v`。
3. 若顯示 `not a git repository`，先找出正確 repository；不要在桌面、OneDrive 根目錄或多人共用工作區直接 `git init`。
4. GitHub 已有 repository 但本機沒有 checkout 時，clone 到明確且不存在的獨立資料夾。
5. fork 常同時有 `origin` 與 `upstream`：推送前確認 `origin` 是使用者自己的 repository，禁止憑名稱猜測 remote。
6. 只加入本次相關檔案，先檢查 diff 與敏感資料，再 commit 並 push 當前正確分支。
7. 只有建立全新 repository 時才執行 `git init`、設定使用者資訊及 `gh repo create --source=. --push`。

回報：登入帳號、repository 根目錄、branch、push remote、commit SHA 與 GitHub URL。
