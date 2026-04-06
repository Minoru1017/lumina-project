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
- **匿名共鳴社群（首頁區塊）**：與 **社群分頁** 共用同一資料來源；動態貼文、「拍拍」加分與粒子回饋；貼文內容支援多行且不外溢卡片。
- **智慧資源推播**：卡片式入口（情境列更新）。
- **成長儀表板**：XP、金幣、今日拍拍數。
- **今日微任務**：完成後標記為已達成並增加 XP（金幣欄位存在，任務獎勵邏輯可再對齊規格）。

### 3. 社群分頁（獨立視圖）

- 與首頁 **互斥顯示**：底部導覽「社群」進入全屏社群體驗（非僅捲動首頁區塊）。
- **留下你的心聲**：多行輸入後發佈，貼文同步出現在首頁與本分頁動態列表底部。
- **獎勵**：成功 **發佈貼文** 或送出 **留言鼓勵回覆** 各獲 **+10 XP**（與儀表板、精靈解鎖規則連動）。
- **留言鼓勵**：於他人貼文（具「留言鼓勵」動線之種子貼文）點擊後展開輸入區與發送；回覆列於該貼文下方；**自己的貼文**不顯示拍拍按鈕。
- **版面**：與首頁社群模組風格一致；手機直向下與首頁 **水平 padding／導覽寬度** 已對齊。

### 4. 精靈分頁（獨立視圖）

- 與首頁 **互斥顯示**：底部導覽「精靈」進入全屏精靈互動區。
- **精靈圖鑑**：儀表標題為「精靈圖鑑」；舞台可 **左右滑動**（或點下方圓點、焦點在舞台時 **←／→**）在 **光精靈** 與 **白瑀鳥** 之間切換。
- **白瑀鳥**：圖鑑用純 CSS 呈現（棉被感身軀、有頭無五官）；解鎖門檻 **45 XP**（`GAME.XP_PET_BAIYU`）。未解鎖時可預覽場景與蒙版說明，達標後蒙版移除、可點擊互動（舞動與音符回饋，仍 **+1 全站 XP**）。
- **白瑀鳥親密度**：解鎖後 **自 0 起另計**（`state.baiyuIntimacyXp`，上限 100 與進度條比例一致）；切到白瑀鳥且已解鎖時，進度條與標籤改顯示該值；光精靈或白瑀鳥預覽時仍顯示與首頁同步的全站 XP。
- **解鎖提醒**：全站 XP **首次達 45** 時會出現 **一次性 Toast**（不重複打擾）。
- **陪伴場景（白瑀鳥）**：圖鑑頁下層有「**鳥媽媽溫暖懷抱**」純 CSS 佈景（暖光、身體與雙翅環抱），白瑀鳥角色在上層可點擊。
- **光精靈**：**場景解鎖**（窗戶／植物／帽子）與首頁規則一致；**外觀與情緒**與首頁同一套；點擊有短暫驚嚇／開心反應。
- **狀態文案**：依當前圖鑑頁籤與解鎖狀態更新（光精靈時與首頁情緒文案同步）。
- **陪伴機制說明卡**：與首頁相同內容之卡片複製於互動區塊下方（含圖鑑／白瑀鳥 45 XP 說明列）。

### 5. 底部導覽與路由邏輯

- **四鍵**：首頁、社群、任務、精靈。
- **首頁／任務**：共用 **同一主視圖**；「任務」以 **捲動錨點** 捲至任務區。
- **社群**、**精靈**：各為 **獨立視圖**（與首頁互斥顯示）。
- 路由鍵使用 `data-nav`，狀態記錄於 `state.activeTab`、`state.homeScrollTarget`。
- **情境列**（Context Pill）文案由導覽按鈕 `data-context` 統一更新，避免多處覆寫不一致。

### 6. 體驗與無障礙相關優化（已實作）

- 視圖切換動畫改為輕量（避免 blur 與捲動動畫同時搶資源），並支援 **`prefers-reduced-motion`**（含切頁、Toast、粒子、背景裝飾、精靈待機動畫、慶祝閃光、親密度條過渡、捲動行為、圖鑑軌道過渡與白瑀鳥動效等）。
- 精靈頁與導覽列的 **底部安全區**、**背景漸層** 與內容 **底部留白** 已調整，減少遮擋與視覺割裂。
- 底部導覽、主內容區多類互動採 **事件委派**（導覽、主題 chips、任務、社群拍拍／軟按鈕、精靈圖鑑點擊等），降低重複綁定風險，便於後續擴充。
- **系統／瀏覽器大字體**：以 **可換行、`min-width: 0`、`rem`／`clamp` 高度** 等讓版面跟著重排；**不**以關閉文字縮放等方式犧牲使用者放大字級（維持無障礙原則）。

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
作品集/網站/          # 工作區根目錄（實務上常在此維護 index.html）
├── index.html
└── Cursor幫忙/
    ├── README.md       # 本說明（專案管理視角）
    └── .cursorrules    # Cursor／AI 協作約定（若存在）
```

---

## 如何預覽

1. 以瀏覽器開啟工作區根目錄之 **`index.html`**（或團隊約定之權威副本）。
2. 或使用本機靜態伺服器（選用）後以瀏覽器開啟對應網址。

> **注意**：若 `Cursor幫忙/` 內另有 `index.html`，請與團隊約定 **唯一權威檔**；本 README 變更紀要以 **2026-04-05** 起之根目錄迭代內容為準撰寫。

---

## 已知限制與後續建議（給排程參考）

- **資料持久化**：尚未接 LocalStorage／後端，重新整理即還原。
- **金幣經濟**：介面有金幣欄位，任務 DOM 具 `data-coin`，是否與完成邏輯掛勾需產品定案。
- **程式結構**：邏輯集中於單檔；規模再長大時可抽離 `app.js`、共用「XP 階段計算」函式以降低重複。
- **無障礙**：可再補導覽 `aria-current`、鍵盤操作精靈互動等。

---

## 版本紀要（近期更新摘要）

### 2026-04-05｜功能交付（驗收對照用）

| 編號 | 類型 | 說明 | 驗收要點 |
|------|------|------|----------|
| F-01 | 新功能 | **社群獨立分頁**：底部「社群」進入專用畫面，與首頁互斥 | 切換首頁↔社群畫面正確；首頁社群區與分頁動態內容一致 |
| F-02 | 新功能 | **發佈貼文**（留下你的心聲）+ **動態列表雙向同步** | 發佈後首頁與社群分頁皆出現同一則貼文 |
| F-03 | 新功能 | **留言鼓勵**：展開輸入→發送→回覆顯示於該貼文下方 | 僅適用設計上含「留言鼓勵」動線之貼文；回覆可見且版面正常 |
| F-04 | 規則 | **XP**：發佈與留言鼓勵各 **+10 XP**；拍拍仍 **+5 XP** | 儀表板 XP 與精靈／場景解鎖隨之更新 |
| F-05 | UX | 發佈區與留言鼓勵輸入框 **預設單行高度**，換行或內容變長時 **自動增高** | 手機直向操作不突兀 |
| F-06 | UI | 「留言鼓勵」按鈕與「拍拍」**視覺重量對齊**；精靈分頁標題字重 **調降** | 肉眼對比舊版可接受 |
| F-07 | 修正 | 手機直向 **社群／首頁 padding 與底部導覽寬度** 對齊 | 不應再出現分頁貼齊螢幕邊而首頁有內縮的割裂感 |
| F-08 | 修正 | 多行貼文 **不溢出** 卡片 padding（首頁與社群分頁） | 長文、換行、標籤列版面協調 |
| F-09 | 移除 | 首頁右下角 **說話小精靈**（氣泡＋角色）整段移除 | 該位置無浮層；無殘留互動 |
| F-10 | 新功能 | **精靈圖鑑**：舞台 **左右滑**／**圓點**／**←→** 切換光精靈與 **白瑀鳥** | 精靈分頁內切換正確；儀表文案隨頁籤更新 |
| F-11 | 新功能 | **白瑀鳥**：**45 XP** 解鎖互動；未解鎖可預覽；解鎖後 **蒙版移除** | 達 `GAME.XP_PET_BAIYU` 後無半透明鎖層；未達時可滑動預覽 |
| F-12 | 規則 | **白瑀鳥親密度** 與全站 XP **分開**：解鎖後自 **0** 起算，僅互動白瑀鳥時累加該條 | 切到白瑀鳥（已解鎖）時進度條與標籤為「白瑀鳥」專用；切回光精靈為首頁同步 XP |
| F-13 | UX | **首次達 45 XP** 時 **一次性 Toast**：提示新精靈／圖鑑解鎖 | 只出現一次（`state.baiyuUnlockToastShown`）；刷新頁面會重置狀態故可能再出現 |
| F-14 | 內容／美術 | **陪伴場景**：白瑀鳥圖鑑頁 **鳥媽媽溫暖懷抱**（純 CSS 下層佈景） | 視覺上環抱小白瑀鳥；不阻擋點擊精靈 |
| F-15 | 內容 | 精靈分頁 **複製「陪伴機制」** 說明卡於互動區下方 | 與首頁卡片內容一致並含白瑀鳥圖鑑列 |
| F-16 | 修正 | 解鎖後蒙版仍顯示：**`[hidden]` 與 `display:flex` 衝突** 已用 CSS 修正 | 解鎖後鎖定層必須完全不可見 |

### 2026-04-05｜工程與維護優化（不變更產品規格之前提下）

| 編號 | 面向 | 說明 | 對專案管理的意義 |
|------|------|------|------------------|
| T-01 | 可維護性 | 遊戲數值集中 **`GAME` 常數**（XP 門檻、拍拍／發佈／鼓勵／點精靈獎勵、Toast 時間等） | 調平衡時改一處即可，減少漏改 |
| T-02 | 效能／穩定性 | **DOM 快取**（導覽列、精靈分頁關鍵節點）；`updateUI` 減少重複查詢 | 行為不變；後續加欄位時較不易拖慢 |
| T-03 | 架構 | **事件委派**擴及主題 chips、任務按鈕（與既有社群委派同一策略） | 新增按鈕較不需複製貼上綁定程式 |
| T-04 | 樣式 | 精靈分頁與多處文案 **內聯樣式改 class**；解鎖物件顯示改 **class 切換** | 視覺對齊設計稿前提下，利於設計共編與除錯 |
| T-05 | 樣式 | **`.btn-surface-pat-like`** 共用拍拍／留言鼓勵按鈕外觀（語意上仍分開，避免誤觸拍拍邏輯） | 統一元件語言，降低 UI 漂移 |
| T-06 | 無障礙 | **`prefers-reduced-motion`** 範圍擴大（見上文 §6） | 符合系統減動效偏好之使用者體驗 |
| T-07 | 無障礙／RWD | **大字體韌性**：頂欄／任務列／統計格／底部導覽／Toast／精靈舞台高度等可換行與伸縮 | 長輩放大字級時較不易破版；原則上不禁止使用者縮放 |
| T-08 | 架構 | 圖鑑相關：**`PET_DEX`**、`state.petDexIndex`、`bindPetDex`（滑動／點擊）、**`updatePetDexChrome`**（蒙版、進度條標籤、儀表文案） | 新增圖鑑精靈時擴充陣列與軌道寬度即可對齊 |

### 更早之前（歷史摘要）

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
- **Anonymous resonance feed (home block)**: shares **one data source** with the **Community tab**; posts, “pat” for XP with particle feedback; multiline post bodies stay inside the card.
- **Gentle resource cards**: entry points (context pill updates).
- **Growth dashboard**: XP, coins, daily pat count.
- **Daily micro-tasks**: completing marks done and adds XP (coins column exists; wiring task rewards to coins is a pending product decision).

### 3. Community tab (separate view)

- **Mutually exclusive** with home: bottom nav “Community” opens a full community experience (not only scrolling the home section).
- **Share your voice**: multiline compose → publish; new posts appear at the bottom of the feed on **both** home and this tab.
- **Rewards**: **+10 XP** for each successful **post** and each **encouragement reply** sent (ties into dashboard and companion unlock rules).
- **Encourage**: on eligible seed posts, tap to open inline input → send → reply renders under that post; **own posts** do not show the pat button.
- **Layout**: aligned with the home community module; on portrait phones, **horizontal padding / nav width** match home.

### 4. Companion tab (separate view)

- **Mutually exclusive** with home: bottom nav “Companion” opens full companion interaction.
- **Spirit encyclopedia (“精靈圖鑑”)**: swipe the **stage** left/right (or use **dots** below, or **Arrow keys** when the stage is focused) to switch between the **light spirit** and **Baiyu bird** (白瑀鳥).
- **Baiyu bird**: unlock interactive mode at **45 XP** (`GAME.XP_PET_BAIYU`). Before unlock, preview with a lock overlay; after unlock, overlay is removed and taps play dance + note feedback while still granting **+1 global XP** per tap.
- **Baiyu-only affinity**: after unlock, a **separate counter** (`state.baiyuIntimacyXp`, 0–100 for the bar) tracks intimacy with Baiyu; when Baiyu is selected and unlocked, the bar + label show that value; otherwise the bar reflects **global XP** (same as home).
- **Unlock toast**: the **first time** global XP reaches **45**, a **one-time toast** announces the new spirit in the encyclopedia (`state.baiyuUnlockToastShown`).
- **Companion scene (Baiyu slide)**: a **warm “mother bird hug”** layer (CSS-only: glow, body, wings) sits **behind** the tappable Baiyu character.
- **Light spirit**: **scene unlocks** (window / plant / hat) and **look & mood** follow home rules; tap “startled” / “happy” reactions unchanged.
- **Status copy**: dashboard text updates per selected encyclopedia entry and unlock state (light spirit mood still mirrors home when that slide is active).
- **“Companion mechanics” card**: same explanatory card as on home is duplicated **below** the interaction block (includes Baiyu encyclopedia line at 45 XP).

### 5. Bottom navigation & routing

- **Four actions**: Home, Community, Tasks, Companion.
- **Home / Tasks** share **one main view**; “Tasks” uses a **scroll anchor** to the tasks block.
- **Community** and **Companion** are each a **separate view** (mutually exclusive with home).
- Nav keys use `data-nav`; routing state lives in `state.activeTab` and `state.homeScrollTarget`.
- **Context pill** text is driven from nav `data-context` in one place to avoid conflicting overrides.

### 6. UX & accessibility-related improvements (implemented)

- Lighter view transitions (avoid heavy blur competing with smooth scroll); **`prefers-reduced-motion`** covers view enter, toast, orbs, ambient blobs, pet idle, celebrate flash, affinity bar transition, scroll behavior, encyclopedia track transitions, and Baiyu motion where applicable.
- **Safe area**, **background gradient continuity**, and **bottom padding** tuned for the companion tab and floating nav to reduce occlusion and visual seams.
- **Event delegation** for floating nav, theme chips, tasks, community actions (pat / soft buttons), and encyclopedia tap handling to avoid duplicate listeners and ease future changes.
- **Large text / browser font scaling**: layout uses **wrapping**, **`min-width: 0`**, and **`rem` / `clamp` heights** so the UI reflows; we **do not** disable user text scaling (accessibility-first).

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
portfolio-site/          # workspace root (often where index.html is maintained)
├── index.html
└── Cursor幫忙/
    ├── README.md       # This document (PM-oriented)
    └── .cursorrules    # Cursor / AI notes (if present)
```

---

## How to preview

1. Open **`index.html`** at the workspace root (or whatever your team names the canonical file).
2. Optionally use a static server and open the URL in a browser.

> **Note**: If `Cursor幫忙/index.html` also exists, agree on **one canonical file**. This README’s **2026-04-05** changelog is written against the **root** `index.html` iteration.

---

## Known limitations & follow-ups (for planning)

- **Persistence**: no LocalStorage / backend yet; refresh clears progress.
- **Coin economy**: UI shows coins and tasks expose `data-coin`; product still decides whether completion should grant coins.
- **Code organization**: logic is monolithic; at larger scale, extract e.g. `app.js` and shared “XP tier” helpers to reduce duplication.
- **Accessibility**: room to add `aria-current` on nav, fuller keyboard support for companion interactions, etc.

---

## Recent changes (summary)

### 2026-04-05 — Feature delivery (acceptance-oriented)

| ID | Type | Summary | Acceptance check |
|----|------|---------|-------------------|
| F-01 | Feature | **Community tab** as its own screen (mutually exclusive with home) | Home ↔ Community switches correctly; feed content matches home block |
| F-02 | Feature | **Publish post** + **bidirectional feed sync** | New post visible on home and community tab |
| F-03 | Feature | **Encourage flow**: inline input → send → reply under post | Only on posts wired for “encourage”; reply visible, layout OK |
| F-04 | Rules | **XP**: +10 for publish and encouragement each; pat stays **+5** | Dashboard XP and unlocks update accordingly |
| F-05 | UX | Compose areas **single-line default**, **auto-grow** on wrap / more text | OK on portrait phones |
| F-06 | UI | Encourage button **visually aligned** with pat; companion tab title **lighter weight** | Visual regression acceptable vs prior |
| F-07 | Fix | Portrait **padding / floating bar width** aligned (home vs tab) | No “full-bleed tab vs padded home” seam |
| F-08 | Fix | Multiline posts **do not overflow** card padding (home + tab) | Long / wrapped text stays inside card |
| F-09 | Removal | Home **bottom-right helper** (bubble + mini companion) removed | No floating widget there |
| F-10 | Feature | **Spirit encyclopedia**: **swipe** stage / **dots** / **arrow keys** to switch light spirit vs **Baiyu** | Works on Companion tab; dashboard copy follows selection |
| F-11 | Feature | **Baiyu**: **45 XP** to unlock interaction; **preview** when locked; **overlay removed** when unlocked | Matches `GAME.XP_PET_BAIYU`; no lock layer after threshold |
| F-12 | Rules | **Baiyu affinity** is **separate** from global XP: starts at **0** after unlock; only Baiyu taps raise that bar | Bar label switches with slide + unlock state |
| F-13 | UX | **One-time toast** when global XP **first reaches 45** | Uses `baiyuUnlockToastShown`; refresh resets in-memory state |
| F-14 | Content / art | **“Mother bird hug”** scene (CSS) behind Baiyu on encyclopedia slide | Warm wrap-around visuals; clicks still hit Baiyu |
| F-15 | Content | **Duplicate “companion mechanics”** card under the interaction block | Same as home card, includes Baiyu encyclopedia note |
| F-16 | Fix | Unlock overlay still visible: **`[hidden]` vs `display:flex`** resolved in CSS | Lock layer fully gone after unlock |

### 2026-04-05 — Engineering / maintenance (no product spec change)

| ID | Area | Summary | PM takeaway |
|----|------|---------|-------------|
| T-01 | Maintainability | Central **`GAME` constants** (XP thresholds, rewards, toast duration) | Balance tweaks in one place |
| T-02 | Performance | **DOM caching** for nav + companion tab nodes; fewer queries in `updateUI` | Same UX; safer to extend |
| T-03 | Architecture | **Delegation** extended to theme chips and task buttons | Fewer one-off `onclick` bindings |
| T-04 | CSS | Inline styles → **classes**; unlock visibility → **class toggles** | Easier handoff with design |
| T-05 | CSS | **`.btn-surface-pat-like`** shared look for pat + encourage (still separate behavior) | Consistent button language |
| T-06 | A11y | Broader **`prefers-reduced-motion`** (see §6 above) | Respects system motion settings |
| T-07 | A11y / RWD | **Large-type resilience** (nav, tasks, stats grid, floating bar, toast, stage height, etc.) | Reduces broken layouts when users enlarge text; no “disable zoom” pattern |
| T-08 | Architecture | Encyclopedia: **`PET_DEX`**, `petDexIndex`, **`bindPetDex`**, **`updatePetDexChrome`** (overlay, bar label, hints) | Add entries + track width when more spirits ship |

### Earlier (historical)

- Full **companion tab** and nav switching.
- **Delegation + `data-nav` + route state**; single exit for context pill.
- Companion **mood / visuals / status** synced with home; tap reactions.
- Companion layout, safe areas, background, **transition performance**.

---

*Purpose of this document: help PM and cross-functional partners understand scope, acceptance criteria, and constraints without reading code. Updated as the prototype evolves.*
