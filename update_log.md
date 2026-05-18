# 更新日誌 — 福岡・九州 旅遊手帳

## 2026-05-18

主公提供新 Excel 行程 source（HK Express 13/06 出發、福岡蒙特埃馬納酒店、13–16/06 細節），對齊 live app。

**行程資料（seed）**
- 全程由 6/15–20 改為 **6/13–20 共 8 日**（回程 6/20 不變 — 主公拍板）。
- 新增 **6/13（六）天神（到埗半日）**：HK Express 10:55–15:20 抵福岡 + 天神散策。
- 新增 **6/14（日）Moff動物咖啡＋寶可夢中心**：Moff Animal Café（ららぽーと福岡4F）· 三井 LaLaport 福岡 · Pokemon Center 福岡（博多丸井0101 2F）。
- 6/15：移走重複嘅「博多丸井2樓」item（已併入 6/14 寶可夢中心），保留 ONE FUKUOKA BLDG。
- 6/16 OUTLETS：交通描述改為 Excel 版「太空世界站步行約5分 + 博多搭 JR 鹿兒島本線區間快速（門司港行）一車直達」。
- 6/17–6/20（由布院/太宰府/運河城/回港）Philips 深度內容原封不動。
- pack 雨具備註日期 6/15–20 → 6/13–20。

**新功能**
- `seed().hotel` schema + hero 下**固定住宿欄**（🏨 福岡蒙特埃馬納酒店 · 渡辺通駅 步行207m · ☎ + 一鍵 Apple Maps 導航）。檢視模式顯示，編輯模式暫隱（v1）。

**UX（諸葛交波 → UU spec → 元芳實作）**
- 全域 `button/.sgo` **press 觸感**：撳落 scale(.95) 回饋，選中日 pill press scale(1.02)，`prefers-reduced-motion` 改用 brightness。root cause = 原本 global `tap-highlight:transparent` 殺咗回饋但全 app 零 `:active`。
- **日期條重設計**：選中日 `scale(1.06)`＋陰影（size 層級而非淨換色）；今日永遠有橙點 `::after`（解決「今日未選＝隱形」）；字 14→16px、min-height 50；兩端 mask 漸隱做 scroll affordance。

**部署**
- `sw.js` VERSION `v3-2026-05-17` → `v4-2026-05-18`（offline-first cache bust）。
- 生成 `#d=base64` 完整匯入連結交主公（開咗即覆蓋部機舊 localStorage — 無後端同步方式，主公拍板）。

**驗證**：node 隔離 eval seed（無 JS error）+ 結構 assertion（8日連續/6/13航班/6/14寶可夢/6/15無重複/hotel/6/16交通）全 PASS + round-trip base64 decode 鏡像 app decoder PASS + `<script>` 整段 `node --check` PASS。⚠️ 真機視覺/觸感未驗（無 browser 環境）→ 需主公 iPhone 實測。
