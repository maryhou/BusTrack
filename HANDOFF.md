# BusTrack — Session Handoff

> 最後更新：2026-07-12（Claude session）

## 專案是什麼

多路線、多目的地的「公車通勤不遲到」PWA。手動輸入「幾分鐘到站＋車程＋步行」，
算出實際抵達時刻，跟該目的地的最晚抵達時間比對（準時／剛好趕上／會遲到），
並在準時班次中標出可以最晚出門的「建議搭這班」。

純靜態、無 build step：`index.html`（全部邏輯內嵌）＋ `manifest.webmanifest` ＋ `sw.js`。

## 目前進度（全部已 commit 到 local main）

- v1.0 核心：多台公車、倒數、遲到判斷、建議班次、可安裝 PWA
- 上車／下車站牌欄位（選填，未填自動隱藏該行）
- 底部 sheet 下滑手勢關閉（跟手、遮罩變淡、110px 門檻或快甩、彈回）
- 卡片 padding 加大（建議標籤不壓到時間）
- 多目的地架構：`bustrack.v2`，destinations[] 各含 name/arriveBy/buses；
  頂部 chips 切換、名稱自動配 icon（家→房子、課/學→學士帽、其他→大樓）、
  點 active chip＝編輯、刪除兩段式確認、v1 資料自動遷移
- 空狀態 emoji 已換成 outline 巴士站牌 SVG

所有功能都在瀏覽器面板用模擬操作驗證過（含 touch 手勢、遷移、刪除流程）。

## 部署狀態：✅ 已上線（2026-07-12）

- 網站：https://maryhou.github.io/BusTrack/（GitHub Pages，main branch 根目錄）
- Repo：https://github.com/maryhou/BusTrack（public；使用者確認 OK 後才建的）
- 之後更新：commit → `git push` 即自動重新部署；記得 bump sw.js 的 CACHE 版號

## ⚠️ 未完成事項

1. 使用者手機實際測試還沒發生（手勢門檻 110px 可能要調）。
2. 未回覆的設計問題：路線號碼牌是深色底白字（仿公車路線牌），使用者偏好
   「忌深色大按鈕」，曾問過要不要改藍色/描邊款，尚無答案。
3. 未來構想（使用者提過、刻意延後）：接 TDX 官方即時到站 API（需付費/金鑰，
   到時要加 Cloudflare Worker 之類的代理，靜態部分不動）。

## 維護須知

- **改 assets 記得 bump `sw.js` 的 `CACHE` 版號**（目前 `bustrack-v2`）
- localStorage schema 改動要在 `load()` 加 migration（參考 v1→v2 的寫法）
- dev server：`.claude/launch.json` 的 `bustrack`（python3 http.server 5180）
- `Icon/` 是設計原始檔（.ai），已 gitignore；網站用的在 `icons/`
- 主題色 `#2563EB` 取自使用者自製 icon；設計偏好：outline icon、毛玻璃、
  白卡大數字（Lyft 風）、繁中介面

## 已知環境怪癖（非 app bug）

- 瀏覽器面板的實體點擊有座標偏移（都落在 (200,820) 附近），測互動請用
  `javascript_tool` 派發事件；touch 手勢可用 `new Touch()`＋`TouchEvent` 模擬
- 預覽瀏覽器的 localStorage 留有測試資料（公司 3 路線＋回家 1 路線）
