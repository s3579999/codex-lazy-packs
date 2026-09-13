---
name: codex-env-setup
description: Codex 環境建置（Node.js, Codex CLI, Git, GitHub CLI, uv）。說「建置環境」「安裝開發環境」時載入。
---

# Codex 環境建置

檢查：在 Windows PowerShell 請分行執行 `node --version`、`codex --version`、`git --version`、`gh --version`、`uv --version`，避免舊版 PowerShell 不支援 `&&`。

| 工具 | Windows | macOS | Linux |
|------|---------|-------|-------|
| Node.js | `winget install OpenJS.NodeJS` | `brew install node` | apt install nodejs |
| Codex CLI | `npm install -g @openai/codex` | 同左 | 同左 |
| Git | `winget install Git.Git` | `xcode-select --install` | apt install git |
| GitHub CLI | `winget install GitHub.cli` | `brew install gh` | apt install gh |
| uv | powershell iex script | `curl -LsSf ... \| sh` | 同左 |

Windows 排錯：

- `npm.ps1` 被執行原則阻擋時，改用 `npm.cmd`。
- 公司網路出現憑證錯誤時，先在當前工作階段使用 `$env:NODE_OPTIONS='--use-system-ca'`；不可永久關閉 SSL 驗證。
- OneDrive 鎖住封裝輸出時，改到新的版本資料夾或 `%TEMP%` 建置，再複製完成品；保留舊版本。

最終驗證所有版本。回報：已安裝／已補裝清單、版本與仍需使用者處理的限制。
