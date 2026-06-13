# 更新日誌 — 福岡・九州 旅遊手帳

## 2026-06-14 — 🎵 福岡音樂之夜（夜晚 live house／爵士整合）

- **目的**：主公旅途中想體驗日本音樂文化；研究天神・大名・今泉 live house 重鎮 + 中洲爵士吧，整合入手帳 6/15、6/19 兩晚。
- **成品（SEED 新增）**：
  - **6/15（一）**「🎵 夜晚音樂（可選・爵士之夜）」item — 主推 Trombone Club（中洲・每晚生爵士 21:00/22:00・週日休・¥1000 charge）＋ Browny；導航 loc 設 Trombone Club。週一大場多休故主打爵士。
  - **6/19（五）**「🎵 夜晚音樂（可選・Live House 之夜）」item — 查實當晚實演：ミニマムジーク@Queblick／MADDOGS@OP's／シャッポ@the voodoo lounge／Neighbors Complain@INSA／WANIMA@福岡サンパレス；導航 loc 設 Queblick（今泉）。
  - **連結頁**新增 3 條：eplus 福岡 live house 查詢 / LivePocket 福岡購票 / Trombone Club 官網。
- **研究來源**：bushikaku（6/19 實演表）、ontaq.jp（確認 TENJIN ONTAQ 係 3 月、今次冇份）、trb-club（爵士時間/收費）、各場官網。
- **驗證**：`node` eval SEED parsed OK（8 日 / 9 連結 / 6-15、6-19 各 2 items）；v2/index.html 已鏡像（與 root identical）；sw v15→**v16-2026-06-14**。
- **⚠️ localStorage seed-once 限制**：SEED 只喺首次開 app（`idx().length===0`）寫入本機，無 migration → **主公而家部手機（6/13 已 seed）唔會自動出現呢兩個 item**。要落部機：app 內 ✎ 編輯 → 管理行程點手動加，或新建行程套範本。即時用：直接撳本對話提供嘅 Google Maps 連結。
- **唔影響**：CRUD / 雲端 / 同步邏輯一律無郁，純 SEED 內容 + 連結 + sw bump。

- **目的**：用主公新整嘅 generic flow-canvas template 為 handbook PWA 自身畫 sitemap + 內部模組圖（協作 / review / demo 用），唔影響 live handbook。
- **成品**：`handbook_flow_canvas.json`（34KB · 71 nodes · 59 edges · 5 flows）— 預備好畀 `~/flow-canvas/flow-canvas.html?p=fukuoka_handbook` 匯入。
- **5 個 flow tab**：
  1. 📱 頁面結構（12 nodes）— App Shell / Header / Date Rail / Hero / 5 tabs / Bottom Sheet / Trip Switcher / Settings
  2. 🎯 功能 / CRUD（15 nodes）— Itinerary CRUD / Photo compress / 導航 deeplink / 開支 aggregate / Packing / Chat / Poll / Trip share / JSON export 等
  3. 🧭 User Journey（18 nodes · 4 flows）— A: 旅程中首次開 / B: 編輯 / C: Pre-trip share / D: On-trip 同行協作
  4. 💾 Data Model（12 nodes）— Trip / Day / Stop / Expense / Link / PackItem / User / ChatMsg / Poll + 3 storage layer buckets
  5. ☁️ Sync & Collaboration（14 nodes）— Capability URL / Firebase anon / onSnapshot / Diff write / SECT / Offline / Rules F1/F4/F5/F6 / Reshare gap
- **Pivot 記錄**：本來 fork BD ESH canvas v2，途中發現主公啱啱整咗 generic template（`~/flow-canvas/`，per MEMORY），即刻轉用，orphan fork 已刪。
- **Node schema 沿用 ESH/template 原樣**：`actor`/`system`/`statutory`/`source` field 重新詮釋為 scope / storage layer / file_ref / source。SYS chip 仍係 BD-flavored (ESS/EPS/...) → handbook 用 Local/Cloud/SW/Mem，chip 顯示字但 fallback 灰色，唔影響可讀。
- **驗證**：JSON parsed clean；5 個 file_ref spot-check 對返 `index.html:49-52` (.pres)、`:54-61` (.drail)、`:64-70` (.hero)、`sw.js:4` (VERSION) — 真實存在。Template 喺 default browser 開咗 `?p=fukuoka_handbook` 等匯入。
- **唔影響**：handbook live (`fukuoka.wicalyu.com`) `index.html` / `sw.js` / `manifest.webmanifest` 一個 byte 都無郁。

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

## 2026-05-19（後續5）— 新手帳 v2 首版（主公命大改UI，馬上交付）

- 新增 `/v2/index.html`：全新設計語言（暖調 editorial、light/auto-dark、卡片+大日 hero、map-forward stop card、collab presence chip、segmented nav），共用 v11 同一行程 seed（8日福岡）。
- root v11 **零改動**（旅程 system-of-record 安全）；v2 獨立 localStorage key `fukuoka2026_v2`。
- 即時可開：https://fukuoka.wicalyu.com/v2/ 。功能：日 rail 切換、stop 完成勾選、開支 JPY→HKD 自動小計、打包勾選、連結。
- 驗證：node --check PASS、headless render smoke PASS（8日/小計/匯率）。⚠️ 視覺/觸感真機未驗（本環境無 browser）→ 你開個 URL 即見；唔 work 我再修。
- v2 北極星後續（presence 即時/真協作/地圖嵌入/journal 相）= post-trip 繼續，今次先交可見可用首版。

## 2026-05-19（後續6）— 新 app 升 root（完整 CRUD + 多行程 + 多用戶 + 每段分享 + 手帳主題）

- v2（多行程 registry / 顯示名多用戶 / 每段獨立 capability-URL 分享 / 完整 CRUD：行程設定·日子·行程點·開支·打包·連結 / UU 手帳主題：紙紋·和紙膠帶日tab·車票存根·DONE 印章）**升做 root**。
- 舊 v11 完整保留喺 `/v1/`（旅程保命備份，icon/manifest/sw 齊）。
- root `sw.js` VERSION → **v12**（殺 cache：之前「未見變化」真兇＝舊 SW serve 緊舊 shell）。
- 驗證：node --check PASS、headless render PASS、root 確認係新 app、/v1/ 備份齊。⚠️ 真機視覺/2機同步未驗（無 browser）→ 開 URL 即見，SW 要一次 reload 先 serve 新版。
