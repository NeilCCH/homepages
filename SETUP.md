# 網站設定說明（重要）

這次更新把「管理者密碼」從網頁原始碼裡移除了，改用 Firebase 官方的登入機制，
並讓 AI 生成文章在正式部署（GitHub Pages）上也能運作。以下 4 個步驟請在
[Firebase 主控台](https://console.firebase.google.com/project/neo-homepages) 完成，
完成後管理與 AI 功能才會生效。

---

## 1. 開啟 Email/密碼 登入，並建立管理者帳號

1. Firebase 主控台 → **Authentication** → **Sign-in method**
2. 啟用 **電子郵件/密碼（Email/Password）**
3. 切到 **Users** 分頁 → **Add user**
   - Email：建議用 `neo0935@gmail.com`
   - Password：**在這裡設定你的管理密碼**（這組密碼由 Firebase 保管，不會出現在網頁原始碼裡）

> 之後在網站上「管理者登入」就是輸入這組 Email + 密碼。忘記密碼時，可在 Firebase
> 主控台重設，不需要改程式。

## 2. 部署 Firestore 安全規則（最關鍵的一步）

登入判斷在瀏覽器端，真正保護資料的是「安全規則」。請部署本專案的
[`firestore.rules`](./firestore.rules)：

- Firebase 主控台 → **Firestore Database** → **規則(Rules)** → 貼上 `firestore.rules`
  的內容 → **發布**。
- 若管理者 Email 不是 `neo0935@gmail.com`，記得把規則裡那一行的 Email 改成你的。

這樣一來：任何人都能「看」文章，但只有你（登入後）能新增／修改／刪除。

## 3. 啟用 Firebase AI Logic（讓「AI 生成草稿」可用）

1. Firebase 主控台 → 左側 **Build** → **AI Logic**（或搜尋 AI Logic）
2. 依畫面指示啟用，後端選 **Gemini Developer API**（有免費額度）
3. 網站用的模型是 `gemini-2.5-flash`

> 若未啟用，網站仍可正常使用，只是按「AI 生成草稿」會跳出提示、需手動輸入內容。

## 4. （建議）開啟 App Check，防止額度被盜用

Firebase 的設定金鑰（apiKey）本來就是公開的，安全性靠上面的「規則」把關。
但 AI 呼叫會消耗你的免費額度，為避免有人拿你的設定亂打，建議開啟 **App Check**：

- Firebase 主控台 → **App Check** → 為你的網站註冊 **reCAPTCHA v3**，
  並對 Firestore / AI Logic 啟用強制檢查（Enforce）。

---

## 本次一併優化的項目

- **檔案體積 6.07 MB → 0.20 MB**：原本把 111 個字型檔內嵌在網頁裡（約 4MB），
  已改為由 Google Fonts CDN 載入 Manrope 與 Noto Sans TC，首次開啟大幅加快。
- **SEO / 社群分享**：補上網頁標題、描述與 Open Graph 標籤（貼到 FB/LINE 會有標題與描述）。
- **管理功能**：文章新增／編輯保留，並新增「刪除」按鈕（僅管理者可見，刪除前會再次確認）。
- **移除指紋登入**：原本的 Face ID / Touch ID 只是前端幌子，擋不住資料庫寫入，已移除以免造成
  誤以為安全。改用 Firebase 登入後，瀏覽器會自動記住登入狀態，重新整理不需再登入。

## 尚待你補充的內容

- 「領導成效」區塊仍有預留字樣「請替換為實際成果數據」，建議換成實際的量化成果
  （例如團隊人數、留任率、業績成長幅度），對專業形象加分最大。
