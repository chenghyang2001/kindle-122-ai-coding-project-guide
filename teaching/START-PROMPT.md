# 接續提示詞

## A. 換到另一台電腦接續（2026-10-08 起用這段）

新筆電先照 `teaching/HANDOFF.md` 第 3 節準備好環境。準備完之後，在 repo 資料夾（`%USERPROFILE%\workspace\kindle-122-ai-coding-project-guide`）輸入 `claude`，貼上：

```
我要在這台電腦接續 K122《讓 AI 寫出好程式！AI Coding 專案開發實戰指南》的互動式教學演練，做法和 K120 一模一樣。原本在家用機做，第 1 章第 1 步的教材已經做好，但我還沒操作。

請先讀：
1. teaching/HANDOFF.md：第 0 節教學做法、第 2 節進度表、第 3 節換電腦說明
2. teaching/LESSON-PLAN.md：本書的特殊做法（用 Claude Code 扮演書中各工具）和各章分步計畫
3. book-summary.md、demo/DEMO-LOG.md：全書重點和書附範例實測結果

先確認環境：python teaching/tools/make_caption_video.py --self-test 要通過，ffmpeg、VLC、edge-tts 都要能用。缺什麼就告訴我怎麼補，不要跳過。

規則照 HANDOFF.md：
- 繁體中文。
- 每步都要有：語音＋同步字幕影片（用 PowerShell Start-Process 開 VLC 播放）、聊天視窗的文字版、teaching/steps/ 底下的說明檔（用記事本開）。
- 一次只講一步。
- 我說 proceed next 時，直接讀這台電腦上練習資料夾的對話記錄來判讀。
- 指出和書上不同的地方，以及原因。
- 每步都更新 HANDOFF.md 的進度表，然後 commit、push。

從第 1 章第 1 步開始：影片已經做好了，請直接用 VLC 重播 teaching/videos/ch01-step1.mp4，並用記事本打開 teaching/steps/ch01-step1.md，然後等我操作。
```

## B. 同一台電腦隔天接續

在 repo 資料夾輸入 `claude`，貼上：

```
我要接續 K122 的互動式教學演練。請先讀 teaching/HANDOFF.md（第 0 節做法、第 2 節進度表）和 teaching/LESSON-PLAN.md，從進度表標示「⏳」的那一步接續。規則照 HANDOFF.md。
```
