# Lumina 微光｜快速互動原型

## 專案定位

單頁式（SPA 風格）前端原型，展示「匿名共鳴社群 × 溫和資源推播 × 遊戲化成長 × 陪伴精靈」的體驗流程。  
**目標讀者**：產品／專案管理、設計、工程——用於對齊範圍、驗收互動、規劃下一階段，而非正式上線產品。

---

## 產品目標（可對齊需求文件）

| 面向 | 說明 |
|------|------|
| 情感與隱私 | 匿名代號、狀態標籤、無真實身分壓力 |
| 參與感 | 社群「拍拍」、任務完成累積 XP、精靈與場景隨成長解鎖 |
| 可擴充性 | 單一 `index.html` 利於快速迭代；後續可拆模組或接後端 |

---

## 功能範圍（目前可驗收項目）

### 1. 引導與個人化（Onboarding）

- 五步驟：歡迎 → 匿名設定與標籤（最多 3 個）→ 心情測驗選夥伴 → 孵蛋動畫 → 完成進入主畫面。
- 進入主程式後，首頁問候、標籤、精靈類型與圖示會依設定更新。

### 2. 主畫面（首頁視圖）

- **主題切換**：清晨／夜晚／療癒（影響全站背景變數）。
- **Hero 與陪伴小窩**：精靈外觀、情緒文案、房間元件（窗戶／植物／帽子）依 **XP 門檻** 解鎖（20／60／100）。
- **匿名共鳴社群**：動態貼文、「拍拍」加分與粒子回饋。
- **智慧資源推播**：卡片式入口（情境列更新）。
- **成長儀表板**：XP、金幣、今日拍拍數。
- **今日微任務**：完成後標記為已達成並增加 XP（金幣欄位存在，任務獎勵邏輯可再對齊規格）。

### 3. 精靈分頁（獨立視圖）

- 與首頁 **互斥顯示**：底部導覽「精靈」進入全屏精靈互動區。
- **親密度進度條** 與首頁 XP 同步（0–100%）。
- **場景解鎖**（窗戶／植物／帽子）與首頁規則一致。
- **精靈外觀與情緒**：與首頁同一套開心／難過邏輯；點擊精靈有短暫驚嚇／開心反應。
- **狀態文案**：儀表板顯示與首頁「光精靈狀態」相同的情緒描述。

### 4. 底部導覽與路由邏輯

- **四鍵**：首頁、社群、任務、精靈。
- 首頁／社群／任務共用 **同一主視圖**，差異為 **捲動錨點**（社群區、任務區）。
- 精靈為 **獨立視圖**；路由鍵使用 `data-nav`，狀態記錄於 `state.activeTab`、`state.homeScrollTarget`。
- **情境列**（Context Pill）文案由導覽按鈕 `data-context` 統一更新，避免多處覆寫不一致。

### 5. 體驗與無障礙相關優化（已實作）

- 視圖切換動畫改為輕量（避免 blur 與捲動動畫同時搶資源），並支援 `prefers-reduced-motion`。
- 精靈頁與導覽列的 **底部安全區**、**背景漸層** 與內容 **底部留白** 已調整，減少遮擋與視覺割裂。
- 底部導覽採 **事件委派**，降低重複綁定風險，便於後續擴充。

---

## 技術說明（給工程／協作）

| 項目 | 說明 |
|------|------|
| 形式 | 單一 `index.html`，內嵌 CSS 與 JavaScript |
| 執行方式 | 以瀏覽器直接開啟檔案即可預覽（無建置步驟） |
| 狀態 | 前端記憶體內 `state` 物件；重新整理頁面後資料重置 |
| AI 協作規範 | 見同目錄 `.cursorrules`（擴充時盡量不破壞既有 HTML/CSS/腳本結構，除非另有約定） |

---

## 檔案結構

```
Cursor幫忙/
├── index.html      # 完整原型（版面、樣式、互動）
├── .cursorrules    # Cursor／AI 協作約定
└── README.md       # 本說明（專案管理視角）
```

---

## 如何預覽

1. 以檔案總管或編輯器開啟 `Cursor幫忙/index.html`。
2. 或使用本機靜態伺服器（選用）：在該資料夾執行任意靜態伺服指令後以瀏覽器開啟對應網址。

> **注意**：工作區根目錄另有一份 `index.html` 時，內容可能與本資料夾不同；**以 `Cursor幫忙/index.html` 為本專案最新原型為準**。

---

## 已知限制與後續建議（給排程參考）

- **資料持久化**：尚未接 LocalStorage／後端，重新整理即還原。
- **金幣經濟**：介面有金幣欄位，任務 DOM 具 `data-coin`，是否與完成邏輯掛勾需產品定案。
- **程式結構**：邏輯集中於單檔；規模再長大時可抽離 `app.js`、共用「XP 階段計算」函式以降低重複。
- **無障礙**：可再補導覽 `aria-current`、鍵盤操作精靈互動等。

---

## 版本紀要（近期更新摘要）

- 新增 **精靈分頁** 完整區塊與導覽切換。
- 導覽改 **委派 + `data-nav` + 路由狀態**，情境列單一出口更新。
- 精靈頁與首頁 **情緒／外觀／狀態文案** 同步；點擊反應與基礎情緒樣式並存。
- 精靈頁版面、安全區、背景與 **切換動畫效能** 優化。

---

*文件目的：讓專案管理與跨職能夥伴在不讀程式碼的前提下，仍能掌握範圍、驗收點與限制。內容隨原型迭代更新。*

---

# Lumina 微光 (Lumina) — Interactive Prototype (English)

## Project overview

A single-page, SPA-style front-end prototype demonstrating **anonymous resonance community × gentle resource recommendations × gamified growth × companion spirit**.  
**Intended audience**: product, project management, design, and engineering — for scope alignment, interaction acceptance, and next-phase planning; **not** a production release.

---

## Product goals (requirements-ready)

| Dimension | Description |
|-----------|-------------|
| Emotional safety & privacy | Anonymous handle, status tags, no real-identity pressure |
| Engagement | Community “pat” interactions, XP from tasks, companion and room unlocks as users progress |
| Extensibility | One `index.html` for fast iteration; can later split modules or add a backend |

---

## Feature scope (current acceptance checklist)

### 1. Onboarding & personalization

- Five steps: welcome → anonymous setup and tags (max 3) → mood quiz to pick a companion → hatch animation → enter main experience.
- After entry, home greeting, tags, companion type, and icon update from onboarding choices.

### 2. Main screen (home view)

- **Theme switcher**: morning / night / calm (updates global background tokens).
- **Hero & companion nook**: companion look, mood copy, and room pieces (window / plant / hat) unlock at **XP thresholds** (20 / 60 / 100).
- **Anonymous resonance feed**: posts, “pat” for XP with particle feedback.
- **Gentle resource cards**: entry points (context pill updates).
- **Growth dashboard**: XP, coins, daily pat count.
- **Daily micro-tasks**: completing marks done and adds XP (coins column exists; wiring task rewards to coins is a pending product decision).

### 3. Companion tab (separate view)

- **Mutually exclusive** with home: bottom nav “Companion” opens full companion interaction.
- **Affinity bar** stays in sync with home XP (0–100%).
- **Scene unlocks** (window / plant / hat) follow the same rules as home.
- **Look & mood**: same happy / sad logic as home; tap triggers short “startled” / “happy” reactions.
- **Status copy**: dashboard shows the same mood line as home “companion status.”

### 4. Bottom navigation & routing

- **Four actions**: Home, Community, Tasks, Companion.
- Home / Community / Tasks share **one main view**; difference is **scroll anchor** (community vs tasks sections).
- Companion is a **separate view**; nav keys use `data-nav`; routing state lives in `state.activeTab` and `state.homeScrollTarget`.
- **Context pill** text is driven from nav `data-context` in one place to avoid conflicting overrides.

### 5. UX & accessibility-related improvements (implemented)

- Lighter view transitions (avoid heavy blur competing with smooth scroll); respects `prefers-reduced-motion`.
- **Safe area**, **background gradient continuity**, and **bottom padding** tuned for the companion tab and floating nav to reduce occlusion and visual seams.
- Bottom nav uses **event delegation** to avoid duplicate listeners and ease future changes.

---

## Technical notes (for engineering / collaboration)

| Item | Description |
|------|-------------|
| Format | Single `index.html` with embedded CSS and JavaScript |
| How to run | Open the file in a browser (no build step) |
| State | In-memory `state` object; refresh resets data |
| AI / Cursor rules | See `.cursorrules` in this folder (prefer not to break existing HTML/CSS/JS structure unless agreed) |

---

## File layout

```
Cursor幫忙/
├── index.html      # Full prototype (layout, styles, interactions)
├── .cursorrules    # Cursor / AI collaboration notes
└── README.md       # This document (PM-oriented)
```

---

## How to preview

1. Open `Cursor幫忙/index.html` from Explorer or your editor.
2. Optionally serve the folder with any static server and open the URL in a browser.

> **Note**: If another `index.html` exists at the workspace root, it may differ from this folder. **Treat `Cursor幫忙/index.html` as the source of truth for this prototype.**

---

## Known limitations & follow-ups (for planning)

- **Persistence**: no LocalStorage / backend yet; refresh clears progress.
- **Coin economy**: UI shows coins and tasks expose `data-coin`; product still decides whether completion should grant coins.
- **Code organization**: logic is monolithic; at larger scale, extract e.g. `app.js` and shared “XP tier” helpers to reduce duplication.
- **Accessibility**: room to add `aria-current` on nav, fuller keyboard support for companion interactions, etc.

---

## Recent changes (summary)

- Added full **companion tab** block and nav switching.
- Nav refactored to **delegation + `data-nav` + route state**; single exit for context pill updates.
- Companion tab **mood / visuals / status copy** synced with home; tap reactions coexist with base mood styles.
- Companion layout, safe areas, background, and **transition performance** refined.

---

*Purpose of this document: help PM and cross-functional partners understand scope, acceptance criteria, and constraints without reading code. Updated as the prototype evolves.*
