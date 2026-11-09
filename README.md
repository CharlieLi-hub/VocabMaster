# VocabMaster Pro — PWA 2.4

這是由你提供的 VocabMaster Desktop v2.4 改寫的 PWA 版本，支援電腦、Android 與 iPhone 主畫面安裝。

## 功能

- 保留原版：單字卡、翻面、朗讀、搜尋、CEFR 篩選、收藏、掌握紀錄、錯題測驗、進度統計、單字編輯、匯入／匯出 JSON、AI 生成主題單字、AI 擴充、AI 自動填寫、AI 造句批改。
- 新增：PWA Manifest、iPhone 圖示、離線快取、手機底部功能列、行動裝置版面調整。
- 安全變更：移除內建 API Key 機制；Gemini Key 僅於「AI 設定」暫時輸入、保存在當前開啟頁面的記憶體，不上傳到 GitHub、不寫入單字備份，也不寫入 localStorage。

## 1. 在 Windows 測試

請在包含 `index.html` 的資料夾開啟終端機：

```bash
python -m http.server 8000
```

用瀏覽器開啟 `http://localhost:8000/`。請勿只雙擊 `index.html`，因為 `file://` 無法測試 Service Worker。

## 2. 發布到 GitHub Pages

1. 登入 GitHub，建立新的 Public Repository，例如 `VocabMaster`。
2. 將這個資料夾內的 **所有檔案及 `icons` 資料夾** 上傳到 Repository 根目錄。請勿上傳舊版填了 API Key 的 HTML。
3. 在 GitHub Repository 開啟 **Settings → Pages**。
4. **Build and deployment → Deploy from a branch → main → /(root) → Save**。
5. 開啟 `https://你的帳號.github.io/VocabMaster/`。首次發布可能需要一點時間。

## 3. 安裝到 iPhone

1. 在 iPhone 用 Safari 開啟 GitHub Pages 網址。
2. 點「分享」，再選「加入主畫面」。
3. 如果看到「以 Web App 開啟」，請保持啟用，按「加入」。
4. 從主畫面點選 VocabMaster 圖示即可使用。

## 4. 原本單字與學習進度如何帶過來？

舊版是在 **原本瀏覽器／原本網址** 的 `localStorage` 裡儲存資料；改成 GitHub Pages 後，因為網址來源不同，不會自動同步。

1. 用你原本的電腦打開舊版 HTML，點 **「完整備份」**，下載 JSON。
2. 在 PWA 裡點 **「匯入」**，選擇剛下載的 JSON。
3. 確認單字數、收藏與進度後，保留一份 JSON 備份。

注意：iPhone PWA、桌面瀏覽器、Safari 分頁彼此的儲存空間不一定共用，**沒有自動同步功能**。請定期匯出備份；移除 PWA 或清除網站資料可能刪除進度。

## 5. Gemini AI 安全說明

- **不要把你的 Gemini API Key 寫到 `index.html`、GitHub 或這份公開專案。**
- 在 App 裡按「⚙ AI 設定」手動輸入 Key。Key 在目前頁面記憶體中，重載或關閉後需重新輸入。
- 現有版本為瀏覽器**直接**呼叫 Gemini API 的個人測試方式，Key 在瀏覽器執行時仍可被檢視或遭惡意程式擷取。多裝置長期使用、分享給他人或上架時，應改用具備身分驗證、額度限制的後端代理（如 Cloudflare Workers），而不是在前端放密鑰。
- 單字卡、測驗、搜尋和進度統計在已快取、且本機資料存在時可離線使用；Gemini AI 功能必須連線。

## 6. 更新版面或功能

將更新後的檔案重新提交到 GitHub；如更新了離線快取資產或 `index.html`，可調整 `sw.js` 裡的 `CACHE_NAME` 版號。重新連線啟動 PWA 後會取得新內容，某些 iPhone 版本可能需要關閉再開啟。**更新前先匯出單字備份。**

此專案未設定雲端資料同步或後端 API 代理，並未代你發布到 GitHub。
