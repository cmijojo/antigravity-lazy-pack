---
name: video-workflow
description: 影片字幕與字卡完整工作流。說「影片工作流」「影片字幕工作流」時載入。
---

# 影片字幕與字卡完整工作流 (Integrated Video Workflow)

根據以下步驟執行：

## STEP 1: 轉字幕
使用「09-audio-to-srt」的 Skill 將音檔轉成逐字稿 (origin.srt)。

## STEP 2: 優化字幕
使用「10-srt-enhancer」的 Skill，參考「origin.md」來優化「STEP 1」的逐字稿，生成 `enhanced.srt`。

## STEP 3: 設計字卡
使用「11-srt-card-annotator」的 Skill，參考「STEP 2」優化後的逐字稿來生成字卡 (reference-cards.srt)。

## STEP 4: 影片介紹
使用「12-srt-social-summary」的 Skill，參考「STEP 2」優化後的逐字稿來生成影片介紹 (summary.md)。
