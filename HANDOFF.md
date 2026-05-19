# HANDOFF — fukuoka 旅行手帳「完成」project

**狀態 2026-05-19**：雲端基建落地，#13 大 build 未做。死線＝真旅程 **2026-06-13 出發**。
恢復一句：「**續 fukuoka #13**」。

## 已鎖定 charter（唔使再問）
- 目標：fukuoka 6/13 前 trip-ready（手感 / 內容定稿 / 功能補完 / 真機離線驗 / 雲端多用戶）。
- 雲端模型 = **capability-URL（Lucy B，主公接受）**：tripId=secret、有 invite link=永久全權、無 revoke（風險已接受）；私人 trip=local-only 永不自動上雲；JSON 匯出=真備份。
- 唔做（defer trip 後）：B3 嵌入地圖、B4 PNG icon。productization 已斬（cards/money 維持免費自用，唔遷 Firebase）。

## 已完成
- Firebase `fukuoka-techo`（個人 acct yuwaiho112@gmail.com，asia-east2/HK）：Web app + Firestore DB。
- `index.html` FB_CFG 已填（web apiKey client-public 非 secret）。
- `firestore.rules` **F8 已 deploy 雲端**（createdBy immutable + migrated 單向）。+ firebase.json/.firebaserc。sw v8。
- 本地 2 commit（`4039bcb` F8、`2b1fb6b` cloud wiring）**未 push**（release ritual：push main 先問 — 主公一句即推）。
- Specs ready：UU #12（press 對位 + 雲端 join/多行程/visibility/同步狀態 UX，誠實無 dark pattern）；Lucy #15（F1 接受/F8 已修/F5/F6 verbatim fix）；元芳 #11（profile data model strawman：namespace 既有 key + 保 seed() shape，前向兼容）。

## 待辦（resume 即做）
1. 🔴 **主公 1 個 console toggle**（自動化唔到，已試 2 API path）：Firebase Console → fukuoka-techo → Authentication → Get started → Sign-in method → **Anonymous → Enable**。未開＝雲端登入 fail（app 自動退單機，唔白屏）。
2. **#13 大 build（atomic 分步，每步 verify）**：F1 刪假 token + F5/F6 rules（coupled，一齊 land 再 deploy）→ 多行程切換器(bottom-sheet) → 私↔公開 deliberate confirm（UU 誠實文案逐字）→ 複製 invite link 全入 URL fragment + 「永久全權收唔返」常駐 caption + 「重開新行程換 ID」逃生門 → join sheet → 同步狀態(同步中/已同步淡出/離線 reassure-not-error/他人更新 toast) → 成員列表(唔造假權限 UI) → JSON 匯出 prominence + 首次上雲 gentle banner → U2 多行程×visibility data model（元芳 #11 strawman）→ invariant：私人永不自動 push。
3. **內容定稿**：PHILIPS_CROSSCHECK 7 條行前確認（丸井2F／太宰府本殿 2026-06 狀態／旅人車資¥610 vs¥700／2026 入梅日／HK Express 6/20 時刻／由布院更新／teamLab）—**事實，要主公或 Philips 確認，元芳唔自填**。
4. **墨子 #14 真機驗收**（cloud 多用戶喺本環境無 browser/第二機驗唔到，必經真機）：離線/PWA裝/2帳號2機 join 同步/capability-URL/私人零雲端/7 條內容對賬。

## Task 冊
#11✅ #12✅ #15✅ · #16 ~90%（差 anon-auth toggle）· #13 pending（主體）· #14 pending（trip-ready 驗收，blocked by #13）
