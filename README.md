# 🧳 福岡・九州 旅遊手帳

單檔可編輯旅遊手帳，部署同台灣卡店地圖一樣（GitHub Pages + PWA 離線）。
**計劃時改、旅行時睇** —— 行程 / 開支 / 連結 / 打包。

## 🔗 Live

**https://wical112.github.io/fukuoka-trip-2026/**

## 點用

- 右上 **👁 檢視 / ✏️ 編輯** 切換：檢視 = 乾淨睇（旅行時用，唔會誤觸）；編輯 = 可加/改/刪/移。
- 4 個分頁：🗺️ 行程（逐日，可勾完成、地點一撳 Apple/Google 導航）· 💴 開支（自動小計 + JPY→HKD，匯率可改）· 🔗 連結 · 🧳 打包（進度）。
- 行程當日會自動標「今日」並捲到該日。
- 改嘢即時存本機（localStorage）。

## 跨裝置（無後端）

- **🔗 分享連結**：複製一條含全部資料嘅網址（`#d=base64`）→ 喺另一部裝置開即匯入（會問確認）。電腦計劃完 → 傳條 link 去電話。改完要重新分享一次。
- **⬇ 匯出 / ⬆ 匯入 JSON**：穩陣備份；資料多到連結過長時用呢個。
- 加主畫面（iPhone Safari 分享鍵）後係 standalone PWA，第二次起**離線都開到**。

## UI（Daylight Travel 設計）

光亮主題（自動跟系統深/淺，戶外日光可讀）· 行程用**時間軸 timeline**（時間 rail + 連接線）· sticky **日期條** 跳日 · **今日/下一站 hero** 一眼睇下一步 · **底部 tab bar**（單手）。設計參考 Wanderlog / TripIt。資料/分享/編輯/PWA 邏輯未動，只重砌 view 層。

## 維護

- 純 client-side、無 build、無後端。種子資料係 `index.html` 內 `seed()`（來自原 Excel 行程截圖），用戶改咗之後以 localStorage 為準。
- **改 code 部署前一定要 bump `sw.js` 的 `VERSION`**（offline-first，唔 bump 用戶 cache 唔更新）。
- 部署：`git add -A && git commit && git push` → GitHub Pages 自動 rebuild（public repo `wical112/fukuoka-trip-2026`，main / root）。

## 未做 / 可加

- 嵌入式地圖（現用外部 Apple/Google 導航 deeplink，較輕、全離線）。
- PNG icon（iOS 對 apple-touch-icon 偏好 PNG，現 SVG 已可用）。
- 多行程切換（現為單一福岡行程）。

資料更新：2026-05-17
