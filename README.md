# 旅跡 Wanderlog

你的 Google Drive 旅遊日記，單一 HTML 檔，部署到 GitHub Pages 即可使用。

---

## 功能
- Google 帳號登入
- 日記、天氣、地圖標記全部存在你的 Google Drive
- 照片上傳至 Google Drive，任何裝置登入皆可顯示
- 景點搜尋（OpenStreetMap Nominatim）
- 列印 / 匯出 PDF

---

## 部署步驟

### Step 1 — 建立 GitHub repo

1. 在 GitHub 建立新 repo（例如 `travel-journal`）
2. 把 `index.html` 上傳（或 git push）
3. 進入 repo → Settings → Pages → Branch: main → Save
4. 記下你的 Pages 網址，例如：`https://yourname.github.io/travel-journal`

---

### Step 2 — 設定 Google Cloud

1. 前往 [Google Cloud Console](https://console.cloud.google.com/)
2. 建立新專案（或使用既有專案）
3. 左側選單 → **API 和服務** → **程式庫**
   - 搜尋 `Google Drive API` → 啟用
4. 左側選單 → **API 和服務** → **憑證**
   - 點擊「建立憑證」→「OAuth 2.0 用戶端 ID」
   - 如果是第一次，先設定「OAuth 同意畫面」：
     - User Type 選「外部」→ 填入應用程式名稱（旅跡）和你的 Email
     - 範圍（Scopes）頁面加入 `https://www.googleapis.com/auth/drive.file`
     - 測試使用者加入你自己的 Google 帳號
   - 回到建立憑證 → 應用程式類型選「**網頁應用程式**」
   - **已授權的 JavaScript 來源** 加入：
     ```
     https://yourname.github.io
     http://localhost
     http://127.0.0.1
     ```
   - 建立後複製 **Client ID**

---

### Step 3 — 填入 Client ID

打開 `index.html`，找到最上方的 `<script>` 區塊：

```js
const GOOGLE_CLIENT_ID = 'YOUR_CLIENT_ID.apps.googleusercontent.com';
```

把 `YOUR_CLIENT_ID.apps.googleusercontent.com` 換成你的 Client ID，再 push 到 GitHub。

---

## Google Drive 資料結構

登入後，app 會在你的 Drive 自動建立：

```
旅跡Wanderlog/
├── journals_index.json          ← 所有旅程清單
├── 北海道之旅_abc123/
│   ├── days_index.json          ← 該旅程所有天的日記資料
│   ├── photo1.jpg
│   └── photo2.jpg
└── 東京五日/
    ├── days_index.json
    └── ...
```

---

## 本機測試

直接用瀏覽器開啟 `index.html` **無法**使用 Google OAuth（安全限制）。  
本機測試請用：

```bash
# Python
python3 -m http.server 80

# 或 Node.js
npx serve .
```

然後開啟 `http://localhost`

---

## 注意事項

- OAuth 同意畫面若為「測試」模式，只有加入的測試使用者可以登入
- 要讓任何人使用，需申請「正式版」發布（填寫 Google 審核表單）
- `drive.file` 權限只能存取 app 自己建立的檔案，不會讀取你 Drive 的其他資料
