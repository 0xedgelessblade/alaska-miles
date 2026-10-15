# 更新紀錄 / Release notes

Changes explain the user problem and the resulting behavior. Live flight data stays on the website. Subsequent releases are recorded in [the release journal](releases/).

## 2026-10-15 — 機型資訊：讓每一段航程都有正確對應

除了出發時間和里程點數，旅客也會想知道搭乘什麼飛機。我們把機型放在航班號旁，用低調的淡綠圓角標籤呈現；轉機則在展開詳情後逐段顯示，不把某一段的機型當成整趟行程。

### 這次處理的難題

- **來源名稱不夠精細，也不能猜：** 保留 `Airbus A321neo/Lr/Xlr` 的原始寫法；只有 `Airbus A350` 的資料也不擅自補成 -900 或 -1000。
- **轉機可能換飛機：** 每段分別保存起訖機場、航班號與機型，驗證航段首尾及轉機點能連接；不單靠轉機次數猜航段結構。
- **中間時間可能缺失：** 允許來源沒有提供的時間為 null，不補造抵達時間、起飛時間或轉機等待時間。已提供的時間則核對時區、先後順序與整體行程。
- **舊資料也要正常使用：** 機型和航段是選填欄位。沒有資料就維持原本畫面，航班ID的計算方式不變，不因補上機型而產生重複航班。
- **資料要真正存得下來：** 同步更新匯入驗證與保存欄位，避免畫面做好了，機型卻在匯入途中被忽略。
- **資訊增加，畫面不能變擁擠：** 保留原本卡片層級；轉機細節展開才顯示，手機版的長機型名稱可換行。

### 完成狀態

網站與匯入服務的功能已部署，96項自動測試通過，桌面與手機版以本機示例驗收。另已逐筆驗證42,315筆含機型測試資料：原有欄位與航班ID未變，驗證沒有錯誤。

**目前正式航班資料尚未匯入這些機型欄位。** 以上描述的是已部署的功能與完成的測試，不代表所有正式航班已經顯示機型。實際機型也可能調整，以航空公司確認為準。

### Aircraft information without guesswork

Direct flights can show a small aircraft label beside the flight number. Connecting itineraries show aircraft by segment in expanded details. We preserve source wording, allow missing timestamps, validate connections and time order, and keep existing flight IDs unchanged.

The importer now retains the optional aircraft and segment fields. Old snapshots remain compatible. The feature is deployed and the aircraft test snapshot has passed validation; production aircraft data is still awaiting import. This update does not claim a particular seat layout or cabin product from an aircraft name alone.

機位資料每日更新，實際機位以航空公司確認為準。

## 2026-10-14 — 讓每次操作都有回應

切換艙等後，如果畫面沒有反應，人會懷疑自己沒按到，或不知道還要等多久。這次從這個小細節開始，讓查機位的過程更安心。

- **按下就有回應：** 艙等按鈕立即呈現選中狀態，旁邊的機位總數改為「查詢中」。
- **等待有說明：** 航班區上方以淡綠圓角提示條顯示正在載入哪個艙等，搭配小型轉圈。
- **畫面保持穩定：** 載入期間保留原有航班的位置，淡化並暫停互動，避免把上一個艙等的資料當成新結果。
- **最後一次選擇優先：** 連續切換時，較慢的舊請求不會蓋過新的選擇。
- **失敗有出口：** 查詢失敗或等待過久時，提供清楚的重試按鈕，不讓人一直看著轉圈。

設計持續打磨的方向很簡單：讓使用者知道自己的操作已被接收、目前發生什麼事，以及下一步能做什麼。提示沿用網站配色，保留鍵盤操作與減少動態效果的支援，不為了回饋額外增加資料請求。會員權限仍由後端驗證。

### Clear feedback for human users

When people change cabins, the page should respond at once. A small loading message says what is happening. The previous flights stay in place but cannot be clicked while new results load. The latest choice wins, and errors offer a way to try again.

We are improving the experience through small, practical changes: less guessing, steady layouts, and clear next steps. Loading feedback does not add data requests or replace server-side membership checks. Fast responses, delayed responses, errors, timeouts, rapid switching, and a mobile layout were checked. This is an interaction improvement, not a claim of a measured speed gain.

## 2026-10-13 — 讓查機位更順：減少等待，保留閱讀位置

- **只載入需要的資料：** 航班列表與詳情不再先取得整張地圖資料；機場選單重用已取得的選項，減少重複請求。
- **載入更多，不打斷閱讀：** 新航班接在列表下方，保留原有航班與已展開的內容。按鈕顯示載入狀態、目前筆數，全部載完後顯示完成提示。
- **登入流程保持直接：** 未登入點航班，立即顯示登入提示。登入後由使用者再點選，不自動跳轉。
- **清楚的回應：** 點選地圖航點時，先顯示航點標題與簡單載入提示，再補上機位資料。

機位資料每日更新，實際機位以航空公司確認為準。

### Simpler browsing, less repeated work

Flight lists and details now skip the extra map request. Airport menus reuse their options. Load more adds flights below the existing rows and keeps expanded details open. Guests see the login prompt immediately and choose their flight again after signing in.

Flight data is updated daily. Actual availability is subject to airline confirmation. Membership checks and data-access limits remain in place. These changes reduce repeated work; no measured speed increase is claimed.

![載入更多航班與目前筆數，原有航班保留在列表中](images/load-more.jpg)

*正式網站功能截圖；其中機位與價格不代表目前庫存。*

## 2026-10-13 — 訪客近期機位

- 新增 `/recent` 分享入口，預設台灣出發、商務艙。
- 訪客可看未來 7 天、4 席以上機位，切換出發／回程及艙等，包含直飛與轉機。
- 沿用近期機位版面，鎖定機場、日期、席位與直飛篩選；點航班先登入。
- 後端強制公開範圍，保留完整航班 API 登入限制與限流。大地圖預覽維持原有範圍。

## 2026-10-13 — 公開作品展示與讀取入口

- 建立 GitHub 作品頁，連結免費地圖預覽、公開 HTML 與 JSON 機位摘要。
- 補上品牌與 Threads 連結、公開讀取及引用指引。
- 每次網站發布建立對應 GitHub 更新紀錄，區分待發布與已發布。

## 2026-10-12 — 更清楚的航線探索

- **降低視覺混亂：** 初次進入聚焦台北至東京經濟艙；可自行展開或收起全部航線。
- **機場辨識：** 收起航線仍顯示外站地名與代碼，統一台灣與外站航點樣式，加入柔和光點。
- **機場選單：** 航班表標題可直接切換兩端機場，保留艙等、日期與席位條件；選單支援鍵盤及手機畫面。
- **同行人數：** 可勾選 2、3、4 位以上，篩選與總數、翻頁一致。
- **手機體驗：** 機位卡移至地圖上方，地圖操作改為主動開啟。
- **公開摘要：** 提供與訪客預覽範圍一致的 HTML、JSON 入口及 Sitemap。

These entries document interface improvements, not airline inventory changes.
