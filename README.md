# 5ch Historical Archive Explorer

ローカルのJSONLアーカイブをSQLiteに取り込み、ブラウザーで検索・集計するアプリです。追加のPythonパッケージは不要です。

## 起動方法

PowerShellでプロジェクトフォルダーを開いて実行します。

```powershell
.venv\Scripts\python.exe app.py
```

ブラウザーで <http://127.0.0.1:5000> を開きます。初回起動時はJSONLをSQLiteへ索引化するため、データ量に応じて時間がかかります。JSONLが追加・更新された場合は、次の起動時またはAPIアクセス時に索引を作り直します。

## データと機能

- 既定の読み込み先: `jsonl/`
- SQLiteデータベース: `archive.sqlite3`（自動生成）
- 投稿本文とスレッドタイトルの全文検索（SQLite FTS5 trigram、日本語部分一致）
- スレッド一覧、投稿表示、月別推移、キーワード頻度、投稿IDの共起ネットワーク
- `date`を保持したまま`date_iso`を生成し、解釈できない日付はNULLとして保存
- `links`、`anchors`、`content_length`、`post_type`を索引データに付与
- 1000レス到達などの運営投稿は削除せず、`post_type=system`として表示
- 空の投稿IDは欠損として扱う

読み込み先や待受ポートは環境変数で変更できます。

```powershell
$env:ARCHIVE_JSONL_DIR = 'D:\archive\jsonl'
$env:PORT = '5001'
.venv\Scripts\python.exe app.py
```

投稿IDの共起は、同じスレッドで観測されたID同士の関係を示します。同一人物だと断定するものではありません。
