# 每日市場簡報 — 設定說明

一個由 Claude Code Routine 每天自動更新的台股導向市場簡報網站，掛在 GitHub Pages 上，保留每日歷史存檔。

## 檔案結構
```
market-brief/
├── index.html              # 首頁：自動列出所有歷史存檔
├── manifest.json           # 存檔索引（routine 每天更新）
├── ROUTINE_PROMPT.md       # 給 Routine 用的提示詞（設定時複製貼上）
├── README.md               # 本說明
└── archive/
    └── _template.html      # 單日簡報模板（routine 讀取後填內容）
```

## 一次性設定步驟

### A. 建立 GitHub repo 並上傳
1. 在 GitHub 建一個新的 repo（例如 `market-brief`），可設為 Public（Pages 免費）或 Private。
2. 把本資料夾所有檔案上傳到 repo。

### B. 開啟 GitHub Pages
1. repo → Settings → Pages。
2. Source 選「Deploy from a branch」，Branch 選 `main`、資料夾選 `/ (root)`，Save。
3. 等一兩分鐘，Pages 會給你一個網址（形如 `https://你的帳號.github.io/market-brief/`）。這就是你之後隨時看的網頁。

### C. 安裝 Claude GitHub App 並連接
1. 確認你的 Claude 方案已啟用「Claude Code on the web」。
2. 前往 claude.ai/code，依指示連接你的 GitHub 帳號，並把上面那個 repo 授權給 Claude（要能 clone 與 push）。

### D. 建立 Routine
1. 前往 claude.ai/code/routines，點「New routine」。
2. 名稱：每日市場簡報。
3. 提示詞：把 `ROUTINE_PROMPT.md` 的全部內容貼進去。
4. Repository：選你剛建的 repo。
5. Trigger：選 Schedule，設每日一次。建議時間：台股盤前，例如 UTC+8 早上 8:00（介面可能以 UTC 顯示，8:00 UTC+8 = 0:00 UTC，請依介面換算）。
6. Connectors：這個任務只用到 web search 與 GitHub，其餘 connector 可移除以縮小存取範圍。
7. 建立後可先按「Run now」跑一次，確認運作正常。

## 注意事項
- **分支限制**：Routine 預設只能推送到 `claude/` 開頭的分支。但 Pages 是讀 `main` 分支，所以你需要在建立 routine 時，為這個 repo 開啟「Allow unrestricted branch pushes」，讓它能直接推 `main`。或者你也可以讓它推到 `claude/` 分支後自己手動合併（較麻煩，不建議）。
- **Pro 方案每日執行額度**：一天跑一次通常不會超限。
- **research preview**：Routines 仍在預覽階段，介面與限制可能調整。
- **權值股清單**：目前固定追蹤台積電、聯發科、鴻海、廣達、被動元件。要改清單，直接修改 ROUTINE_PROMPT.md 步驟 1 與步驟 3 的對應標的，重新貼回 routine 提示詞即可。
- **首次首頁是空的**：manifest.json 初始為空陣列，routine 第一次跑完後才會出現存檔。

## 驗證
Routine 跑完後：
1. 開你的 Pages 網址，首頁應出現「最新一期」卡片與歷史列表。
2. 點進去應看到當天完整簡報。
3. repo 的 commit 紀錄應有一筆「每日市場簡報 YYYY-MM-DD」。
