# K122 書附範例演練紀錄

- 日期：2026-10-08
- 機器：家用機（Windows 10，Git Bash），Python 3.12（uv venv），Flask 3.0.0（照 `requirements.txt`）
- 範例：`code/HAM-Radio-Practice-Web`（commit `ccdf4e3`），複製到 `demo/work/ham-app` 執行。app 會直接寫入 `data/questions.db`，所以不在 submodule 裡跑
- 執行紀錄：`demo/logs/ch04-08-smoke.log`、`demo/logs/flask-run.log`

## 環境準備

```bash
cp -r code/HAM-Radio-Practice-Web demo/work/ham-app && rm -rf demo/work/ham-app/.git
cd demo/work/ham-app
uv venv --python 3.12 .venv
uv pip install --python .venv -r requirements.txt
.venv/Scripts/python.exe app.py        # http://127.0.0.1:5000
```

資料庫現況：`questions` 411 題（答案 0/1/2/3 分別是 110／101／111／89 題）；`sessions` 已有 135 筆、`question_sets` 已有 5460 筆，都是作者開發時留下的測驗紀錄。

## 情境測試（Flask test client，以檔案路徑載入 `app.py`）

| # | 情境 | 結果 | 判定 |
| --- | --- | --- | --- |
| 1 | 35 題全部答對 | session 136，35 題不重複，`Passed Test`、100%，0.27 秒 | ✅ |
| 2 | 35 題全部答錯 | `Failed Test`、0% | ✅ |
| 3 | 剛好答對 26 題 | `Passed Test`、74%（及格線） | ✅ |
| 4 | 同一題 POST 兩次（使用者按重新整理） | 計分變成 2 題，下一題索引跳過一題 | ❌ 重複計分 |
| 5 | 沒選答案就送出 | HTTP 400 Bad Request（`request.form['answer']` 的 KeyError） | ⚠️ 沒有友善提示 |
| 6 | `GET /delete-cookie` | HTTP 500：`The view function for 'before_request' did not return a valid response` | ❌ |
| 7 | `GET /quiz/XXXX/1`（不存在的題號） | HTTP 200，顯示空白題目 | ⚠️ 應回 404 |
| 8 | 沒有 cookie 直接開 `/results` | HTTP 200，顯示「沒有作答」 | ✅ 可接受 |
| 9 | 清空 `sessions`／`question_sets` 後建立第 1 個 session | session 1 **沒有寫進 `sessions`**，答題不計分，永遠停在第 2 題 | ❌ |

另外：

| # | 項目 | 結果 |
| --- | --- | --- |
| 10 | `flask --app app run` | `Failed to find Flask application or factory in module 'app'`。`app/` 套件（有 `__init__.py`）把 `app.py` 遮住了；Python 裡 `import app` 也拿到套件 |
| 11 | `pytest` | `tests/test_routes.py` 是空檔，沒有任何測試 |

## 找到的 bug 與根因

1. **第 1 個 session 不會寫入（情境 9）**
   `Session.create_session()` 的 `INSERT` 寫在 `else` 分支裡。`MAX(session_id)` 是 `None` 時只設 `session_id = 1`，沒有 INSERT。之後的 `UPDATE ... WHERE session_id = 1` 都更新 0 筆，答題數永遠是 0。作者的資料庫一開始就有資料，所以沒發現。這很可能就是第 6.1 節「發現第一個 bug」的同類問題（〔推論〕）。
2. **`/delete-cookie` 500（情境 6）**
   `@app.route('/delete-cookie')` 和 `@app.before_request` 疊在同一個函式上。函式裡比對的是 `request.endpoint == 'delete-cookie'`，但實際 endpoint 名稱是函式名 `before_request`，所以永遠不成立、回傳 None。另外 `response.delete_cookie()` 也少了 key 參數。前端其實改用 JavaScript 刪 cookie（`results.html`），這條路由是死碼。
3. **重新整理會重複計分（情境 4）**
   `store_answer()` 沒有檢查這一題是否已經作答過。「下一題」是用「已答題數」當索引去 `question_sets` 取，所以多算一次就會跳過一題。
4. **`app.py` 與 `app/` 同名（項目 10）**
   只能用 `python app.py` 啟動。改名成 `run.py`，或把 Flask 物件移進 `app/__init__.py`（application factory）即可解決。
5. **其他品質問題**
   - 每個路由都寫死 `db_path = 'data/questions.db'`，所以要在專案根目錄執行。`config.py` 和 `app/routes.py` 是空檔。
   - `app.run(debug=True)` 開著 debugger，不可用於正式環境。
   - 到處 `print` 除錯訊息。
   - `get_next_question()` 在 35 題以上的分支會引用未定義的 `next_question`。
   - `question_sets` 只有 `question_set_id` 自動遞增，`create_question_set` 的 `question_set_id` 參數沒有用到。

## 給互動教學的對照重點

- 第 5～6 章的實作，學員自己做的版本可以和上面 9 個情境對照，看 AI 寫的程式有沒有同樣的 bug。
- 第 8 章請 AI 生成測試時，好的測試應該要能抓出 bug 1～3。
- 書上的流程（設計文件 → 擷取需求 → stub → 實作）在範例裡留有痕跡：`grader.py`／`question_selector.py` 一直是 stub，從來沒有被用到。
