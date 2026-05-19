# 更新日誌 — 福岡・九州 旅遊手帳

## 2026-05-19（後續4）— 雲端共享 UX 成品（取代 stopgap）

- **成品**：bottom-sheet 共享 UX 取代舊 confirm/prompt stopgap。涵蓋：開共享/經連結加入(顯示名)/複製邀請連結+常駐「永久全權收唔返」誠實告知/成員 live 列表(無造假權限)/「重開新行程換 ID」逃生門(capability 唯一 de-facto revoke)/離開共享/JSON 匯出 prominence + 「雲端非備份」提示。
- 同步狀態：header pill（同步中/已同步/離線）+ 離線 persistent banner（reassure 非 error）+ 同行者改動 toast；online/offline listener；cloudPersist/startCloudSync/cloudBoot 已 wire。
- 新控件全繼承全域 button press primitive；sheet 有 Esc/backdrop 關 + focus + reduced-motion。
- **Scope 誠實**：多行程切換器(B2) 明確不做（行程得福岡一個、非 6/13 必需），唔係靜雞雞 drop。
- 驗證：node --check PASS；18 個 cloud fn identifier 全在；seed 無回歸(8日)；cloud rules 早前已 live 驗證。⚠️ 視覺/觸感/2-機同步實際操作 **本環境無 browser 驗唔到 → 墨子 #14 真機**（非 stub，係完整實作待真機收貨）。sw v10→v11。

## 2026-05-19（後續3）— LIVE check：F6 delete 缺陷修正

- **Live headless 驗證**（真 anon signUp + 直打 live Firestore）：F1 capability / F5 invites-deny / F6 section 白名單 / F8 createdBy-immutable + 正常可改 title + 無 world-list(F7) — **全部 live 守到**。Anon Auth 真 work。
- 🐞 **Live check 揾到真缺陷**：F6 原 `request.resource.data is map` 套落 `allow write`，令 DELETE（無 resource.data）被擋 → 合法 member 清唔到 state doc（trip cleanup 會累積 orphan）。node 邏輯測測唔到，live 實測先現形。
- **修正**：F6 拆 `allow create,update`（限4 section+map）vs `allow delete`（只 isMember）。已 redeploy + live 重驗 state delete 200 PASS。
- 所有 test 資料已清（含一個 client 清唔到嘅 orphan，修 F6 後清除）。

## 2026-05-19（後續2）— 內容定稿：Philips 行前 7-gap 重查

- 太宰府本殿釋疑：seed 6/18 改「本殿 2026-05-17 已重開、大改修完」，pack「行前查本殿」todo 釋除（✅ d:true）。
- 太宰府車資：改建議直接買 1-day pass ¥2,100，免單程 ¥610/¥700 之爭。
- HK Express 6/20：加 ⚠️「以你 booking confirmation 為準」警告（single-source）。
- Pokémon Center 2F / 入梅指引：Philips 驗證確認原內容準確，無需改。
- 待主公決定 placement（gap-first 未自動入行程）：teamLab Forest 福岡、由布院シャガール美術館翻新。
- PHILIPS_CROSSCHECK.md ledger 已補 2026-05-19 重查表。sw v9→v10。

## 2026-05-19（後續）— #13 capability-URL 重構 + Lucy F1/F4/F5/F6

- **F1**：刪走假 invite token（`fbNewToken` 移除、create/join 唔再寫/讀 invites doc）；tripId(20-char 不可猜)=唯一 capability secret，rules 本來就係咁。
- **F4**：`fbInviteUrl` 改 `#t=<tripId>`（全入 fragment，唔入 query → 唔經 Referer/server log/歷史顯眼處）；`tripCtx` 仍兼容舊 `?t=`。
- **F5**：`firestore.rules` invites collection 一律 deny（已無用）。
- **F6**：state 寫入 rules 限定 section ∈ {itinerary,expenses,packing,meta} + 須 map（封垃圾 doc 灌 quota；client defensive validate 屬 #13 待續項）。
- cloudMenu stopgap 加「連結=永久通行證收唔返」誠實告知；rules F5/F6/F8 已 deploy 雲端。
- 驗證：node `--check` script PASS；tripCtx/fbInviteUrl round-trip（fragment-only/no-token/legacy-?t=/localStorage 四案）PASS；rules brace+F5/F6/F8 結構 PASS。⚠️ 真機 / 多帳號多機同步 **本環境無 browser 驗唔到 → 墨子 #14**。
- sw v8→v9。**未做（#13 待續大件）**：UU 正式 sheets UX（多行程切換器/visibility deliberate confirm/同步狀態 UI/成員列表/JSON 匯出 prominence）、client snapshot defensive validate、內容定稿 7 條、Anonymous Auth console toggle（主公）。

## 2026-05-19 — 雲端共享啟用 #16（capability-URL 模型 B）

- Firebase project `fukuoka-techo`（個人 account yuwaiho112@gmail.com、asia-east2/HK）建好；Web app + Firestore DB 起好。
- `index.html` FB_CFG 填入真 config（Firebase web apiKey 設計上 client-public，非 secret）。
- `firestore.rules` **F8 已 deploy 到雲端**（createdBy immutable + migrated 單向，Lucy #15）；加 `firebase.json` + `.firebaserc`。
- sw.js VERSION v7→v8（index.html 改動）。
- ⚠️ **未完（誠實）**：Anonymous Auth provider 試 Identity Toolkit API 兩次失敗（CONFIGURATION_NOT_FOUND — 全新 project Auth 未初始化，無 CLI/API 繞過）→ **需主公 Firebase Console 一個 toggle**：Console → fukuoka-techo → Authentication → Get started → Sign-in method → Anonymous → Enable。未開呢個，雲端登入會 fail（app 自動退單機，唔白屏）。
- F1/F5/F6 + #13 client 重構（capability-URL 刪假 token、多行程 UX、同步狀態）未做 — 大 build 待續；雲端多用戶只能墨子 #14 真機驗。

## 2026-05-18（後續 2）— 安全 meta（Lucy 安全方案 T2-2）

- `<head>` 加 `<meta name="robots" content="noindex">`（charter: unlisted）+ `<meta name="referrer" content="no-referrer">`。
- 分享連結用 `#hash` fragment（已確認正確，唔經 Referer 洩漏，無需改）。
- sw.js `VERSION v5→v6`（index.html 有改動，強制刷 PWA cache）。

## 2026-05-18（後續）— 搬去自訂 domain

- 新增 `CNAME` → `fukuoka.wicalyu.com`（GitHub Pages 自訂 subdomain）。
- `index.html` og:url / og:image 由 `wical112.github.io/fukuoka-trip-2026/` 改為 `https://fukuoka.wicalyu.com/`。
- sw.js VERSION `v4` → `v5`（index.html 有改動，強制刷 PWA cache）。
- PWA 用相對路徑（`./`），搬去 subdomain root 不受影響。
- 待辦：GoDaddy 加 CNAME `fukuoka` → `wical112.github.io`，GitHub Pages 設 custom domain + Enforce HTTPS。

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
