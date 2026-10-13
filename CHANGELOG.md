# 更新紀錄 / Release notes

Changes explain the user problem and the resulting behavior. Live flight data stays on the website. Subsequent releases are recorded in [the release journal](releases/).

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
