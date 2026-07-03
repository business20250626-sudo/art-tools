# AI 生成工具教學網站

美術用的 AI 生成工具（Leonardo、Higgsfield）教學與評測站，純靜態 HTML，直接用瀏覽器打開即可，不需伺服器或安裝。

## 頁面

| 檔案 | 導覽名稱 | 內容 |
|---|---|---|
| `index.html` | Leonardo 教學 | Leonardo 介面與各功能（Library / Image / Video / 3D / Blueprint / Upscaler / Flow State）教學，含工作坊練習。 |
| `Leonardo選模型.html` | 圖片模型評測 | 互動式選模型工具：過濾條件 + 用途矩陣，幫你依需求挑圖片模型。 |
| `評測樣本.html` | 圖片評判標準與樣本 | 圖片模型的評判 rubric，以及 Phase 1/2/3 全部實測樣本（Phase 3 每張圖標了套用的風格）。 |
| `影片評測.html` | 影片模型評測 | Leonardo 影片模型 Phase 1（畫面品質）+ Phase 2（動作品質）實測與評分。**目前設為「即將推出」，內容以 HTML 註解暫時隱藏。** |
| `問答紀錄.html` | 問答紀錄 | 美術實際問過的問題與解法知識庫，可搜尋、分「🖼️ 圖片 / 🎬 影片」兩個 tab。 |

導覽列還有一個 `Higgsfield`，標「即將推出」尚未建立頁面。

## 資料夾

- `thumbs/` — 圖片評測用的縮圖（約 172 張），`評測樣本.html` 引用。
- `影片評測/` — 影片評測用的 mp4（Phase 1 各模型 t1/t2、Phase 2 各模型 p2）。
- `教學圖/`
  - `leonardo/` — `index.html` 教學用截圖。
  - `問答紀錄/` — `問答紀錄.html` 用的圖片與影片（見下方命名規則）。
- `評測進度筆記.md` — 評測過程的原始筆記（工作備忘，非網頁內容）。

## 共用設計

所有頁面沿用同一套深色主題（CSS 變數 `--bg / --panel / --accent …`）與置頂導覽列。改導覽時，**5 個頁面的 `<nav>` 要一起改**（index / Leonardo選模型 / 評測樣本 / 影片評測 / 問答紀錄）。

- 圖片路徑一律用**相對路徑**（`教學圖/…`、`thumbs/…`），不要加開頭斜線 `/`，否則用 `file://` 直接開會找不到檔。
- 圖片槽（`figure.shot`）都有 `onerror` fallback：檔案還沒放時顯示虛線佔位框，放進去就自動顯示。

## 問答紀錄：如何新增一則

`問答紀錄.html` 裡 `<div id="list-img">` 上方有註解說明。重點：

1. 複製一整個 `<article class="qa"> … </article>`，放進對應分頁：
   - 圖片問題 → `<div id="list-img">`
   - 影片問題 → `<div id="list-vid">`
2. 改標題、標籤 `<span class="tag">`、日期 `data-date`、內容。
3. 標題慣例：**功能 / 關鍵字在前 + 冒號 + 簡短說明**，例如 `Image Reference：用參考圖生成想要的風格 / 角色`。
4. 搜尋會自動掃描整張卡片文字，不需額外設定；tab 上的則數也會自動計算。

### 內建區塊

- **步驟**：`<ol class="steps">`，每個 `<li>` 內放 `<div class="st-t">步驟文字</div>` + 圖片槽。
- **圖片槽**：
  ```html
  <figure class="shot">
    <img src="教學圖/問答紀錄/檔名.png" onerror="this.style.display='none';this.nextElementSibling.style.display='flex';">
    <div class="ph" style="display:none">放圖片：說明<br><code>教學圖/問答紀錄/檔名.png</code></div>
  </figure>
  ```
- **影片槽**（放 mp4）：
  ```html
  <video class="vid" controls preload="metadata" src="教學圖/問答紀錄/檔名.mp4"></video>
  ```
- **可複製 prompt 框**：
  ```html
  <div class="prompt"><button class="cp" onclick="copyPrompt(this)">複製</button>你的 prompt</div>
  ```

### 圖檔命名規則（`教學圖/問答紀錄/`）

每則問答一個前綴：`p1-`（圖片第 1 則）、`p2-`（圖片第 2 則）、`v1-`（影片第 1 則）、`v2-`（影片第 2 則）…副檔名 png / jpg / mp4 皆可，記得 `src` 要跟實際副檔名一致。

## 恢復「影片模型評測」頁

目前 `影片評測.html` 內容被 HTML 註解包住、導覽列標「即將推出」。要重新公開：

1. 移除 `影片評測.html` 裡的佔位 `<main>` 與註解標記（`<!-- ===== 內容暫時隱藏 … -->` 到 `===== /隱藏區 ===== -->`）。
2. 把 5 個頁面導覽列的 `<a class="soon">影片模型評測</a>` 改回 `<a href="影片評測.html">影片模型評測</a>`。
