---
name: designer-taste-pro
description: 強制 Agent 在生成前端網頁時，套用頂尖設計審美與微動效。當使用者說「設計審美」「套用設計」「設計風格」「美化網頁」「UI/UX 設計」時載入。
---

# 頂尖設計審美與前端開發規範

本 Skill 旨在引導 Agent 擺脫平庸、模板化的 AI 設計（AI Slop），套用符合頂尖設計工作室標準的視覺美學、字型排版與細節動效。

## 零、動工前的設計契約 (Design Contract)

在撰寫任何前端 UI 程式碼前，Agent 必須先明確定義或與使用者對齊以下四個設計維度。若使用者未主動提及，Agent 須根據需求先行推導並於對答開頭進行簡短聲明：

1. **Purpose (目的)**：此介面的核心任務是什麼？（例如：展示專案、填寫表單、數據分析）
2. **Audience (受眾)**：誰在使用？他們進來最想優先看到什麼資訊？
3. **Tone (調性)**：
   * *Utilitarian (實用高效)*：緊湊、高資訊密度、清晰的視覺階層（如 SaaS 後台）。
   * *Editorial (雜誌感)*：大字級標題、大片留白、精緻襯線或幾何字型（如個人品牌、部落格）。
   * *Expressive (前衛表現)*：大膽的配色、打破網格的重疊排版、霓虹發光或玻璃擬態（如產品 Landing Page）。
4. **Memorable Detail (獨特記憶點)**：此介面哪一個視覺元素最讓人印象深刻？（例如：卡片邊框微光 hover、頁首動態玻璃模糊、點擊時按鈕的實體擠壓感）

---

## 一、字型系統 (Typography)

拒絕使用瀏覽器預設字型或單一 Inter。請依專案氣氛在 HTML/CSS 中引進以下免費商用字型：

### 1. 標題用字型 (Headings)
- **`Clash Display`** (現代、幾何、時尚科技感)
  - CDN 載入方式：
    ```html
    <link href="https://api.fontshare.com/v2/css?f[]=clash-display@200,300,400,500,600,700&display=swap" rel="stylesheet">
    ```
- **`Plus Jakarta Sans`** (溫和、現代、具親和力)
  - Google Fonts 載入方式：
    ```html
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    ```

### 2. 內文用字型 (Body)
- **`Satoshi`** (極佳的可讀性、精緻的新無襯線字體)
  - CDN 載入方式：
    ```html
    <link href="https://api.fontshare.com/v2/css?f[]=satoshi@300,400,500,700&display=swap" rel="stylesheet">
    ```
- **`Geist Sans`** (由 Vercel 開發，冷冽、工程與科技感)
  - CDN 載入方式：
    ```html
    <link href="https://cdn.jsdelivr.net/npm/geist@1.3.0/dist/font/sans.css" rel="stylesheet">
    ```

---

## 二、色彩與材質 (Colors & Materials)

- **深色模式 (Dark Theme)**：背景優先使用 `#080B10` 或 `#0B0F19` 等極深藍灰色，而非純黑。
- **高質感卡片 (Glassmorphism)**：使用半透明背景搭配邊框與背景模糊效果：
  ```css
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  ```
- **漸層設計**：避免 `from-blue-500 to-purple-600` 等高飽和度粗俗漸層。應使用 `cubic-bezier` 概念或網格發光點（Glow effects）來做視覺引導。

---

## 三、微動效與互動 (Micro-interactions)

- **轉場曲線 (Easing)**：禁用預設的 `linear` 或簡單的 `ease-in-out`。
  統一使用 **Ease-Out Expo** 曲線，讓動效起步極快、收尾極柔和：
  ```css
  transition: all 0.35s cubic-bezier(0.16, 1, 0.3, 1);
  ```
- **Hover 互動**：
  - 卡片 hover 時微微上移 2px 至 4px，並伴隨羽化半徑擴大的投影。
  - 按鈕 click 時使用 `transform: scale(0.98)` 創造按壓反饋。
