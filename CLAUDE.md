# CLAUDE.md

給 Claude Code 用的專案導覽。每次在這個目錄啟動時會自動讀取，目標是不用重新探索就能直接開始改東西。

## 專案說明

林鳳凰果園（嘉義縣竹崎鄉家族果園）的品牌形象官網，單頁式（one-page scroll）行銷網站，非電商、不含購物車/金流。

**頁面結構（`index.html` 內的 section，依序）：**
1. `#top` Hero — 主視覺大圖 + 標語
2. `#about` 關於我們 — 三代果園故事
3. `#quality` 品質保證 — 產銷履歷驗證、生產追溯、培訓精進、嘉義在地直送，四張卡片（`.quality-grid`，4欄→手機2欄）
4. `#products` 產品介紹 — 「當季鮮果」＋「加工品」兩組。龍眼（大金剛/水貢潤蒂仔/早生/十月龍眼）、荔枝（糯米糍/黑葉/桂味/玫瑰紅）用大卡片 `.product-card`（有品種列表+多張圖）；其餘商品（香蕉、橘子、砂糖橘、黃金果、葡萄柚、人心果、仙桃、酪梨、桑葚、紅柚、龍眼乾、桑葚汁）用小卡片 `.product-simple-card`（`.product-simple-grid` 容器，僅單圖或佔位、無品種細分）
5. `#contact` 聯絡我們 — Facebook 社團連結（可點擊）／電話／LINE／產銷履歷 QR 徽章圖，四欄 `.contact-grid`

**目前內容缺口（改動前先確認是否仍未補齊）：**
- 電話、LINE 聯絡方式目前顯示「聯絡方式更新中，敬請期待」（`.contact-placeholder`），尚未取得使用者確認的真實聯絡方式，**不要自行填入猜測的號碼或連結**
- Facebook 已填：`https://www.facebook.com/groups/925737937782079/`（注意這是 FB **社團**不是粉專，文案用詞需保持一致）
- 以下 9 項商品完全沒有素材照片，用 `.placeholder-media` + emoji 佔位：橘子、砂糖橘、黃金果、葡萄柚、人心果、仙桃、酪梨、桑葚、紅柚。新增照片時直接把對應 emoji 卡片的 `.placeholder-media` 換成 `<img>` 即可，不用改版面結構

**部署：**
- GitHub repo：https://github.com/Cyao0406/linfenghuang-orchard
- 線上網址（GitHub Pages）：https://cyao0406.github.io/linfenghuang-orchard/

## 技術架構

純前端 vanilla HTML/CSS/JS 靜態網站，**沒有框架、沒有建置工具、沒有 npm 依賴**，改完檔案直接生效，不用 build。

```
index.html    單頁全部內容（所有 section 都在同一份檔案）
css/style.css 所有樣式，design tokens 定義在最上方 :root
js/script.js  只做兩件事：footer 年份自動更新、手機版漢堡選單開關
images/       依商品分中文子資料夾（images/龍眼/、images/荔枝/、images/香蕉/、images/龍眼乾/、images/contact/），
              資料夾內檔名為英文語意化命名（如 images/龍眼/cluster.jpg）
```

`js/script.js` 很短，不要為了小改動加框架或建置流程，維持純手刻的風格。

## 風格規範

**色彩系統（`css/style.css` 的 `:root` design tokens）：**
```
--color-forest:      #2d4a34   標題、品牌主色（深綠）
--color-leaf:         #4a7c59   輔助綠
--color-amber:        #d98a3d   強調色（按鈕、eyebrow 文字、TAG）
--color-amber-dark:   #b8702c   強調色 hover/深版
--color-cream:         #faf6ee   主背景（暖白）
--color-cream-dark:   #f1e9d8   區塊交錯背景
--color-text:          #2b2b26   正文
--color-text-soft:    #5a5a52   次要文字
```
新增顏色一律先定義成變數放進 `:root`，不要在規則裡直接寫死 hex。整體色調是「森林綠 + 琥珀橘 + 暖奶油白」的果園/在地感，新增視覺元素要延續這個色調，避免飽和度過高的鮮豔色。

**字型：** 系統中文字型堆疊（`-apple-system, PingFang TC, Microsoft JhengHei, Noto Sans TC...`），沒有外掛字型檔，維持零外部字型請求。

**視覺語言：**
- 大圓角：卡片 16–20px，按鈕 999px（全圓角膠囊）
- 陰影：柔和、帶品牌色透明度（例：`box-shadow: 0 12px 32px rgba(45, 74, 52, 0.1)`），不要用純黑陰影
- Section 之間用 `--color-cream` / `--color-cream-dark` 交錯當背景，製造分段感，不用分隔線
- 標籤文字（`.section-eyebrow`）全大寫 + letter-spacing，作為每個 section 的小標

**RWD：** 單一斷點 `860px`（手機版漢堡選單在此切換），行動版 section padding 從 96px 收成 64px。

**內容佔位符慣例：** 素材還沒到位時不要留空或刪掉整塊，照下面模式處理：
- 圖片未到位 → `.product-simple-card` 內用 `.placeholder-media` + 對應 emoji（見 index.html 內各商品卡片），不加「照片準備中」文字（emoji本身已足夠表意，維持卡片精簡）
- 聯絡資訊未到位 → `.contact-placeholder` class +「更新中，敬請期待」
- **絕對不要**為了填滿版面自行編造電話、LINE、統編、品種口感描述等未經使用者確認的資訊——寧可留佔位，也不要塞假資料

## 常用指令

```bash
# 本機預覽（專案內沒有 package.json，直接用 python 開靜態伺服器即可）
cd "C:\Users\user\OneDrive\桌面\30-39 Work & Projects\35 Family Business\林鳳凰果園\website_2026"; python -m http.server 8800
# 開 http://localhost:8800

# 部署（改完內容/樣式後）
git add -A; git commit -m "說明這次改了什麼"; git push
# push 後 GitHub Pages 會自動重新部署，通常 1-2 分鐘生效
```

**Git 慣例：** 每完成一個獨立改動（例如「補上荔枝照片」「填聯絡電話」）就各自 commit + push，方便之後單獨回退某一次改動，不要把多個不相關的內容更新囤在同一個 commit。
