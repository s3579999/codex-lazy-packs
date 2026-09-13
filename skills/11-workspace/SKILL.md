---
name: codex-workspace
description: Codex 新專案初始化與既有專案安全維護工作模式。說「初始化專案」「老師建專案」「維護既有專案」「開工」「收工」「Windows 建置」時載入。
---

# 專案初始化與安全維護（Codex 版）

## 先判斷工作類型

- 新專案：詢問名稱、用途、明確的專案子資料夾、GitHub repository、公開／私有與部署需求。
- 既有專案：先讀專案根目錄 `AGENTS.md`、`PROJECT_STATE.md` 或等效狀態文件，再確認使用者指定範圍。
- 不可把「維護既有專案」當成「重新初始化」；不可在桌面、雲端硬碟根目錄或多人共用工作區直接 `git init`。

## 新專案

1. 建立或補齊 `AGENTS.md`、`README.md`、`.gitignore`、標準資料夾與必要的 GitHub／Obsidian 入口。
2. 建立前先盤點；已有檔案只補缺口，不覆蓋既有內容。
3. 確認 `startup-sync`、`shutdown-sync`、`project-init-sync` 是否已在 `~/.codex/skills/`。

## 既有專案維護

1. 先讀狀態文件，不全面掃描 repository；只讀需求、入口與本次真正會修改的檔案。
2. 保留既有資料格式、使用者未提交變更與舊版成果；不做無關重構。
3. 修改後只執行直接相關測試；失敗要修根因並重跑相關回歸。
4. 更新狀態文件，精簡記錄完成項目、尚未完成、涉及檔案、驗證與已知問題，不貼完整程式碼。

## Windows 版本交付

1. 每個版本使用新的 `outputs/windows/<程式名稱> <版本號>/`，不可覆蓋或混放舊版。
2. 檔案寫入與更新採暫存檔、內容驗證、flush／fsync、原子替換；需要時先建立可復原備份。
3. 先跑定向測試再封裝；封裝後確認 FileVersion／ProductVersion。
4. 雲端同步資料夾內的 EXE 應複製到本機暫存位置做啟動冒煙測試，避免同步鎖定干擾判斷。
5. 文件要渲染後逐頁檢查；若最後重建任一成果，需在最後一次建置後重新產生並複核 SHA-256 清單。

## 開工/收工

開工：使用 `startup-sync`，讀 AGENTS.md → 讀狀態文件／Obsidian 筆記 → 確認正確 Git 根目錄與 remote → 回報。

收工：使用 `shutdown-sync`，檢查敏感資料與 diff → 更新狀態／筆記 → 只提交本次相關檔案 → 確認 remote 後 push → chezmoi 同步（見 13-chezmoi）。

回報：修改檔案、測試／建置結果、交付路徑、GitHub URL、commit SHA 與未驗證限制。
