# K122 互動教學交接文件

> 建立日期：2026-10-08
> 機器：家用機（Windows 10，Claude Code，Opus 5.5）
> 書：《讓 AI 寫出好程式！AI Coding 專案開發實戰指南》（Jeremy Morgan 著，旗標 2026/06）
> 做法：和 K120 一模一樣（語音＋同步字幕影片 → 使用者實作 → 判讀），差別見 `LESSON-PLAN.md` 開頭的「本書的特殊做法」

---

## 0. 教學做法（沿用 K120 確立的規則）

1. **一律繁體中文。**
2. **每一步都要有語音和字幕**：
   - 先寫旁白稿 `teaching/narration/chNN-stepX.txt`。用口語寫，不放表格和符號，因為 TTS 念出來會很怪。
   - 再用 `teaching/tools/make_caption_video.py <旁白稿> --out teaching/videos/<同名>.mp4 --title "<標題>"` 產生影片。
   - 用 PowerShell `Start-Process` 開 VLC 播放。Git Bash 用 `&` 背景啟動會失敗，VLC 不會出現：
     `powershell -NoProfile -Command "Start-Process -FilePath 'C:\Program Files\VideoLAN\VLC\vlc.exe' -ArgumentList '--play-and-exit','--no-video-title-show','\"<mp4 Windows 路徑>\"'"`
   - 每步另寫說明檔 `teaching/steps/chNN-stepX.md`，用 `notepad.exe` 開啟（終端機顯示會截斷）。
   - 聊天視窗也附文字版。
3. **每次只講一步**，講完就停，等使用者回報。
4. **使用者通常不貼結果，只說 proceed next**。判讀時直接讀練習資料夾的對話記錄：
   `~/.claude/projects/C--Users-<使用者>-workspace-<練習資料夾名>/*.jsonl`。開頭是「你是一個嚴格的記憶萃取器」的 sdk-cli session 要略過。再加上實際檢查檔案、git log、port。
5. **使用者用 cmd**：給的指令用 `%USERPROFILE%`、`set VAR=1`，不要用 `$env:`。
6. **請使用者自己打指令或貼提示詞**，不要把整段教學說明貼給練習 session（K120 的 2-5 就因此破壞了對照組）。
7. 判讀時要分清「書上的行為」和「使用者全域設定造成的行為」：全域有 writer→QA→reviewer 鐵律、commit 後 push、Stop hook。學習情境可以選**快速 demo 模式**（`~/.claude/instructions/learning-fast-mode.md`）。
8. 要講出**和書上不同的地方以及原因**。本書換工具（Claude Code 扮演 Copilot、ChatGPT 等），所以還要說明「工具差異」和「流程差異」。
9. Git Bash 的 PATH 沒有 ffmpeg 時，先執行：
   `export PATH="$LOCALAPPDATA/Microsoft/WinGet/Packages/Gyan.FFmpeg_Microsoft.Winget.Source_8wekyb3d8bbwe/ffmpeg-9.0.2-full_build/bin:$PATH"`（版本號以實際資料夾為準）
10. 每步做完：更新本文件第 2 節進度表 → commit → push。

---

## 1. 對照資料

| 檔案 | 用途 |
| --- | --- |
| `book-summary.md` | 全書 10 章重點，每段標明出處（簡介／Manning／照片／範例／演練／推論） |
| `demo/DEMO-LOG.md` | 書附範例實測：9 個情境、5 個 bug（第 5、6、8 章的判讀標準） |
| `code/HAM-Radio-Practice-Web` | 書附完成版（submodule，**不要修改**；要跑就複製一份出來） |
| `teaching/LESSON-PLAN.md` | 第 1–10 章分步計畫 |

---

## 2. 目前進度

| 章 | 步 | 狀態 | 備註 |
| --- | --- | --- | --- |
| 1 | 第 1 步：LLM 不是資料庫——不存在的函式（1.5 節） | ⏳ 影片已播放，等使用者操作 | 旁白／影片／說明：`ch01-step1.txt`／`.mp4`／`teaching/steps/ch01-step1.md`；練習資料夾 `k122-ch01-practice` |
