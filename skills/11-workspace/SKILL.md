---
name: codex-workspace
description: Codex 新專案初始化與既有專案安全維護工作模式。說「初始化專案」「老師建專案」「維護既有專案」「開工」「收工」「Windows 建置」「免安裝版」時載入。
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

### Electron 封裝判斷

- portable EXE 是單檔免安裝版，但第一次啟動可能先自解壓到暫存目錄；NSIS `Setup` EXE 才是安裝版。檔名與說明文件必須清楚區分。
- 複製 portable EXE 不會自動搬移 `localStorage`、JSON、附件或備份；跨電腦前先確認程式的資料路徑與匯出／還原方式。
- electron-builder 的 signing 訊息不代表已有可信簽章；用 `Get-AuthenticodeSignature` 驗證最終 EXE。未簽章檔可能被 SmartScreen、端點防護或公司政策記錄／攔截，不得保證隱匿。
- OneDrive 鎖定建置成果時，先停止已核對路徑的測試程序，改到 `%TEMP%` 封裝，再複製到新的版本資料夾。
- 公司 TLS 攔截下，Node.js 優先在目前 PowerShell 工作階段使用 `$env:NODE_OPTIONS='--use-system-ca'`；不可把 `strict-ssl=false` 寫進永久或全域設定。

### 既有 UI 修改驗證

- 既有按鈕失效先查事件與 computed style；注意後置 CSS 規則可能覆蓋響應式 `hidden`，不要新增第二個重複按鈕掩蓋根因。
- 顯示／排序偏好使用獨立設定鍵，僅改畫面，不可改寫原始業務資料。
- 拖曳功能要實際驗證視覺回饋、插入位置、套用、取消、重設與重啟保存；桌面與手機寬度都要操作並檢查 console。
- 除單元測試外，至少跑 build、lint、Electron 主程序語法檢查及真實 UI 回歸。

## 開工/收工

開工：使用 `startup-sync`，讀 AGENTS.md → 讀狀態文件／Obsidian 筆記 → 確認正確 Git 根目錄與 remote → 回報。

收工：使用 `shutdown-sync`，檢查敏感資料與 diff → 更新狀態／筆記 → 只提交本次相關檔案 → 確認 remote 後 push → chezmoi 同步（見 13-chezmoi）。

回報：修改檔案、測試／建置結果、交付路徑、GitHub URL、commit SHA 與未驗證限制。
