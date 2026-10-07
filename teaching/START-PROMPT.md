# 接續提示詞（換 session 或換電腦時用）

在 repo 資料夾（`%USERPROFILE%\workspace\kindle-122-ai-coding-project-guide`）裡輸入 `claude`，貼上：

```
我要接續 K122《讓 AI 寫出好程式！AI Coding 專案開發實戰指南》的互動式教學演練，做法和 K120 一模一樣。

請先讀：
1. teaching/HANDOFF.md：第 0 節教學做法、第 2 節進度表
2. teaching/LESSON-PLAN.md：本書的特殊做法（用 Claude Code 扮演書中各工具）和各章分步計畫
3. book-summary.md、demo/DEMO-LOG.md：全書重點和書附範例實測結果

規則照 HANDOFF.md：繁體中文；每步要語音＋同步字幕影片（Start-Process 開 VLC）＋聊天文字版＋teaching/steps/ 說明檔（記事本開）；一次只講一步；我說 proceed next 時，直接讀練習資料夾的對話記錄來判讀；指出和書上不同的地方與原因；每步更新 HANDOFF.md 進度表並 commit、push。

從進度表標示「⏳」的那一步開始。
```

## 新電腦

照 K120 的 `teaching/HANDOFF.md` 第 3 節準備環境：winget 裝 Git、Python 3.12、uv、FFmpeg、VLC、Node，再 `python -m pip install edge-tts`。接著 clone 本 repo：

```
git clone --recurse-submodules https://github.com/chenghyang2001/kindle-122-ai-coding-project-guide
```

驗證：`python teaching/tools/make_caption_video.py --self-test` 要印出 `SELF_TEST_PASS`，而且要實際產生一支影片才算通過。
