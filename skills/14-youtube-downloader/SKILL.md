---
name: youtube-downloader
description: 下載 YouTube 影片、音訊或字幕。當使用者提到「下載 YouTube」「抓 YouTube 影片」「下載影片」「yt-dlp 下載」「抓字幕」時觸發。
---

# YouTube 影片與字幕下載工作流 (YouTube Downloader Skill)

本技能使用系統預裝的 `yt-dlp` 工具執行下載作業。

## 常用下載指令模式

### 1. 抓取中文字幕並轉為 Markdown / SRT（推薦字幕整理流）
```bash
yt-dlp --skip-download --write-sub --write-auto-sub --sub-langs "zh-TW,zh-Hant,zh-Hans,en" --convert-subs srt -o "%(title)s.%(ext)s" "影片網址"
```

### 2. 下載最高畫質影片 (自動合併最佳影音為 MP4)
```bash
yt-dlp -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]/bv*+ba/b" -o "%(title)s.%(ext)s" "影片網址"
```

### 3. 下載 1080p 影片（兼顧畫質與檔案大小）
```bash
yt-dlp -S "res:1080,ext:mp4:m4a" -o "%(title)s.%(ext)s" "影片網址"
```

### 4. 只下載純音訊 (轉 MP3)
```bash
yt-dlp -x --audio-format mp3 --audio-quality 0 -o "%(title)s.%(ext)s" "影片網址"
```

### 5. 批次下載播放清單
```bash
yt-dlp -o "%(playlist_index)02d - %(title)s.%(ext)s" "播放清單網址"
```

---

## 執行規範
1. 檔名包含特殊字元時，需自動過濾 `/ \ : * ? " < > |` 等非法字元。
2. 字幕下載後，若需要整理為 Markdown，需去除 WEBVTT 時間標籤與重疊冗餘行。
3. 批次抓取字幕時，各請求間隔建議保留 1.5 秒（`time.sleep(1.5)`）以防觸發 YouTube 429 限制。
