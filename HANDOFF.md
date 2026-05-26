# HANDOFF · 旅遊手帳（暫時封存 snapshot · 2026-05-26）

恢復一句：「**續手帳**」即接 — `~/fukuoka-trip-2026/` · live `https://fukuoka.wicalyu.com/`（sw v15）· root commit `fd18f01`。

## 已 live 嘅完整 app（v2 為 root）

**架構**
- `index.html`（=v2）+ `sw.js v15`（cache-bust 用）+ `manifest`+`icon` + `v2/index.html`（鏡像 v2 來源）+ `v1/`（舊版完整保命備份）+ `firestore.rules`（已 deploy fukuoka-techo）+ `firebase.json/.firebaserc`
- 設計：手帳主題（紙紋／和紙膠帶日 tab／車票存根 fly+move／DONE 蓋印／封面布紋 header）；OKLCH-ish 暖調；light + auto dark
- 雲端：Firebase project **fukuoka-techo**（個人 acct `yuwaiho112@gmail.com`，asia-east2/HK）· Anonymous Auth ON · capability-URL 模型（Lucy F1/F4/F5/F6/F7/F8 live-verified）

**已落實功能（皆 live）**
1. **多行程**：頂部 ▾ 切換／開新（範本或空白）／改名／刪；每段獨立 localStorage `fk2_t:<id>` + 共用 registry `fk2_idx`/`fk2_act`
2. **多用戶**：右上設顯示名（localStorage `fk2_me`），共享時同行者見到
3. **每段獨立分享**：每段一鍵上雲（自家 cloudTripId），複製邀請連結 `#t=<tripId>` · 永久通行證誠實告知 · 重開換 ID 逃生門 · 離開／停止共享
4. **完整 CRUD（全 inline，非 prompt）**：✎ 編輯掣切換態
   - 行程點：每卡 ✎改/🗑刪 · 每日尾 ＋加行程點（表單：名/時間/類別/地點/花費/備註）
   - 本日：✎ 改本日資料 / 🗑 刪本日 / ＋ 加一日（按日自動排序）
   - 🧭 行程設定：名/開始/結束/匯率/酒店全套表單
   - 開支：手動項 ✎🗑 + ＋加開支（行程點 cost 自動併入、標來源）
   - 打包：每項 ✎🗑 + ＋加項（檢視模式可勾選）
   - 連結：每項 ✎🗑 + ＋加連結
5. **行程點放相**：📷 加相（canvas 壓 1280 / JPEG 0.6）· lazy thumb · 撳開全屏 · 編輯模式 × 刪
6. **同行（chat + poll）**：5th tab 「💬 同行」
   - 討論：實時 chat（Firestore `trips/{tid}/chat`，member-gated rules deployed）· **📎 附相多媒體**（壓縮 + 預覽 chip + 一鍵發送，msgimg lazy）
   - 投票：開題 + 多選項 + 即時 bar，runTransaction 防衝突
7. **同步狀態**：header pill 同步中/已同步/離線 · 離線 banner（reassure 非 error）· 同行者改動 toast
8. **動畫／反應**：panel rise + stop/li/poll stagger + 全域 press + reduced-motion 守規
9. **Lazy loading**：thumbs+msgimg `loading="lazy" decoding="async"` · firebase SDK defer · inline script 先本機 render 即時可見 · 雲端等 `load` 後 attach

**安全姿態**：capability-URL（tripId = acknowledged secret，無 revoke 重開換 ID 自救）· 主公明確接受 · Lucy 已 live-verify rules（F1/F4/F5/F6/F7/F8 + chat/polls member-gated 已 deploy）· F4 invite link 全 fragment

**Memory 記錄**：[[reference-wicalyu-domain]] · [[project-fukuoka-trip-2026]] · [[feedback-autonomy-bias-to-action]]（升級指令：唔再 ceremony，馬上執行）· [[feedback-user-no-self-deploy]]（agent 自己 deploy）

## 未閉環（resume 時優先）

1. 🔴 **墨子 #14 真機驗收**（trip-ready sign-off）— 物理上要主公部 iPhone + 另一機/帳號跑：開新行程／2-機共享同步／capability-URL／discussion+poll／放相／離線回覆／PWA 裝。本環境無 browser/第二機，呢個係唯一硬閘。
2. 🟠 主公心目中可能仲有未提嘅功能（影片/語音/Firebase Storage、地圖嵌入、journal 相簿 view、presence 即時顯示「邊個睇緊邊日」）— 北極星仲有，按需開單。
3. 🟢 ~/fukuoka-trip-2026/v2/ 同 root 為鏡像；以後改一定要兩邊一致（or 留 v2/ 做來源、deploy script 自動 copy）

## 重要操作備忘

- `firebase deploy --only firestore:rules --project fukuoka-techo`（rules 改完）
- `cp v2/index.html index.html && sed -i '' bump sw.js VERSION && git add -A && git commit && git push`（每次 ship root 嘅 ritual）
- 用戶要清 cache：sw 已 skipWaiting+claim，kill app 重開一次即食新版

## 5-min 真機 smoke（俾你做）

開 `fukuoka.wicalyu.com` →（1）撳標題 ▾ 切換／＋開新行程（2）右上設你顯示名（3）撳「分享」→ 複製連結 → 第二部機/瀏覽器開 → 加入 → A 改行程 → B 應幾秒見更新 + toast（4）✎ 編輯 → 改行程點/加日/改設定（5）行程點 📷 加相 → 撳開全屏（6）💬 同行 → 傳訊息 + 📎 附相 + 開投票投票（7）熄網一陣 → 改嘢 → 復網 → 應自動補同步

報哪步爆我即修。
