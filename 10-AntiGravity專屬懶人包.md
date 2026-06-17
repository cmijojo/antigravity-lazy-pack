# Anti-Gravity 懶人包 #10：影片字幕與字卡工作流

> 版本：v1.0
> 更新日期：2026-06-17
> 語系偏好：繁體中文（Taiwan）

這份懶人包的目標，是為 Anti-Gravity 使用者提供一個專門處理「影片字幕與字卡」的 AI Agent 工作流。本工作流支援將音檔自動轉為 SRT 逐字稿、比對原稿（Markdown）進行拼字與空格優化、在字幕中智慧標註字卡，並最終自動產生適合 Facebook、Thread、YouTube 的影片介紹與貼文。

---

## 先備條件

- [ ] 已安裝 Python 3.7+
- [ ] 已安裝 `ffmpeg`（音訊處理核心依賴，請確保可在終端機執行）
- [ ] 已安裝 `openai-whisper` 與 `pydub` 等 Python 依賴套件

macOS 快速安裝：
```bash
brew install ffmpeg
pip install openai-whisper pydub
```

Windows 快速安裝（使用 Chocolatey 或手動下載）：
```powershell
choco install ffmpeg
pip install openai-whisper pydub
```

驗證環境：
```bash
ffmpeg -version
python -c "import whisper, pydub; print('✅ 套件載入成功')"
```

---

## 一、音訊轉字幕 (09-audio-to-srt)

將常見的音訊格式（MP3, WAV, M4A, FLAC 等）轉換為符合規範的 SRT 字幕檔。

### 執行指令
在含有音訊檔的資料夾下執行 Python 轉譯腳本：
```bash
python scripts/audio_to_srt.py <your_audio_file> [--max-chars 22]
```
*   `--max-chars`：每行字幕最大字數限制（預設 22，最少 4）
*   **輸出檔名**：`origin.srt`

### 核心原則
- 自動檢查 Python 與 ffmpeg 依賴環境。
- 自動填補時間軸空隙（小於 0.3 秒的間隔將會自動合併或展延）。
- 不把大型音訊檔或中途的 Whisper 模型檔提交到 Git。

---

## 二、字幕優化 (10-srt-enhancer)

利用 AI（Agent）比對原始文字稿（`origin.md`）來修正 `origin.srt` 中的錯字、專有名詞與排版。

### 核心原則
1.  **時間軸不變**：絕對不能修改任何時間軸與字幕編號。
2.  **內容忠實度**：僅修正現有字幕，不可從 Markdown 中塞入新字幕或段落。
3.  **空格規範**：在中文與英文/數字交界處，自動加上**半形空格**（例如：`Gemini 3.5`）。
4.  **輸出檔名**：`enhanced.srt`

---

## 三、字卡標註 (11-srt-card-annotator)

AI 會分析字幕語意，在字幕下方智慧新增字卡標記，格式為 `[字卡](word:重點字詞 / type:字卡類型)`。

### 字卡類型與樣式

| 類型 (type) | 適用情境 | 範例 |
| :--- | :--- | :--- |
| **金色重點** | 核心結論、關鍵金句 | `[字卡](word:AI已經自動搞定 / type:金色重點)` |
| **白色提醒** | 建議、補充說明、備註 | `[字卡](word:請在同目錄放原稿 / type:白色提醒)` |
| **紅色警告** | 踩雷、風險、錯誤防範 | `[字卡](word:不可修改時間軸 / type:紅色警告)` |
| **藍色列點** | 清單、步驟、平行結構 | `[字卡](word:步驟一音訊轉字幕 / type:藍色列點)` |

### 標註原則
- 每 10 段字幕中，至少要標註 2 個字卡。
- 字卡詞彙長度控制在 10-16 個字，且必須是字幕中實際出現的詞彙。
- **輸出檔名**：`reference-cards.srt`

---

## 四、社群媒體摘要生成 (12-srt-social-summary)

自動分析 `enhanced.srt` 內容，生成適合三大社群平台的吸睛介紹文案：

-   **Facebook**：口語化、強調價值點、結尾附 CTA，約 100-250 字。
-   **Thread**：精簡、強力 Hook、引起討論的疑問句，1-2 個 Hashtag。
-   **YouTube**：SEO 友善、前三行吸睛、大綱結構、包含潛在時間戳記。
-   **輸出檔名**：`summary.md`

---

## 五、一鍵自動化工作流 (13-video-workflow)

在 Anti-Gravity 中，你可以直接下達此指令以啟動完整影片字幕與字卡工作流：

```text
/gen-video-srt-card-des
```

### 流程步驟
1.  **STEP 1**：呼叫 `09-audio-to-srt` 將音檔轉成 `origin.srt`。
2.  **STEP 2**：呼叫 `10-srt-enhancer` 比對 `origin.md` 優化生成 `enhanced.srt`。
3.  **STEP 3**：呼叫 `11-srt-card-annotator` 為優化後的字幕標註字卡，生成 `reference-cards.srt`。
4.  **STEP 4**：呼叫 `12-srt-social-summary` 生成社群推廣文案 `summary.md`。

---

## 常見問題與排除 (FAQ)

| 問題 | 可能原因 | 排除方式 |
| :--- | :--- | :--- |
| **轉字幕時顯示 `ffmpeg not found`** | 系統 PATH 中找不到 ffmpeg | 確保已安裝並設定環境變數，重啟終端機。 |
| **優化字幕時把整段原稿塞進來了** | Agent 忘記了 Content Fidelity 原則 | 請在 Prompt 提醒它：「僅修正 srt 現有字詞，不要加入原稿其他部分」。 |
| **中文與英文中間沒有空格** | 正則表達式或 AI 排版漏掉 | 請提醒 Agent 重新執行 `srt-enhancer` 檢查空格規範。 |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
| :--- | :--- | :--- |
| 2026-06-17 | v1.0 | 初版建立，完整移植音訊轉字幕、字幕優化、字卡標註與社群摘要四核心工作流與相關 Skill |
