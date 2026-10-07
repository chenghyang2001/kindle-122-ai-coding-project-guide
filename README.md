# Kindle 122《讓 AI 寫出好程式！AI Coding 專案開發實戰指南》範例演練

旗標《讓 AI 寫出好程式！AI Coding 專案開發實戰指南》（Jeremy Morgan 著，施威銘研究室編譯，2026/06，ISBN 978-986-312-871-7；原文 Manning《Coding with AI: Examples in Python》）的書附範例演練與互動教學紀錄。

這本書沒有電子書版本，手上沒有內文。章節整理根據天瓏的簡介與完整目錄、Manning 官網，以及書店實拍的照片，再加上書附範例實際執行的結果。

## 目錄

| 路徑 | 內容 |
| --- | --- |
| `book-summary.md` | 全書 10 章整理，每段標明來源：〔簡介〕〔Manning〕〔照片〕〔範例〕〔演練〕〔推論〕 |
| `code/HAM-Radio-Practice-Web/` | 書附範例（git submodule，[JeremyMorgan/HAM-Radio-Practice-Web](https://github.com/JeremyMorgan/HAM-Radio-Practice-Web)），貫穿第 3～8 章的 Flask＋SQLite 業餘無線電執照練習測驗 |
| `demo/DEMO-LOG.md` | **範例實測：9 個情境、5 個 bug 與根因** |
| `demo/logs/` | 執行紀錄 |
| `teaching/` | 互動教學（語音＋同步字幕影片）：`HANDOFF.md`、`LESSON-PLAN.md`、`START-PROMPT.md`、`narration/`、`videos/`、`steps/`、`tools/` |

## 快速開始

```bash
git clone --recurse-submodules https://github.com/chenghyang2001/kindle-122-ai-coding-project-guide
cd kindle-122-ai-coding-project-guide

# 範例會改寫 data/questions.db，先複製一份再跑
cp -r code/HAM-Radio-Practice-Web demo/work/ham-app && rm -rf demo/work/ham-app/.git
cd demo/work/ham-app
uv venv --python 3.12 .venv && uv pip install --python .venv -r requirements.txt
.venv/Scripts/python.exe app.py          # http://127.0.0.1:5000（不能用 flask run，見 DEMO-LOG）
```

## 重點發現

- 主流程正常：35 題隨機不重複，答對 26 題（74%）及格。
- **空資料庫建不出第 1 個 session**：`create_session()` 的 INSERT 放在 `else` 裡，測驗會永遠做不完。
- `/delete-cookie` 一律回 500：`@app.route` 和 `@app.before_request` 疊在同一個函式上。
- 重新整理同一題會重複計分，而且會跳過一題。
- `app.py` 與 `app/` 套件同名，所以 `flask run` 找不到 app。
- `tests/` 是空的。這正好是第 8 章「用 AI 生成測試」的練習對象。

範例程式碼的著作權屬於原作者與出版社（MIT License），本 repo 只用於個人學習。
