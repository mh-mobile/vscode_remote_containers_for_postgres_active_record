# Vscode Remote Containers for ActiveRecord (PostgreSQL)

ActiveRecordの[バグ報告テンプレート](https://github.com/rails/rails/blob/master/guides/bug_report_templates/active_record_gem.rb)をVSCodeのリモートコンテナーで使えるように移植したコードです。SQLite3の代わりにデータベースにPostgreSQLを使います。

ホストOSにDockerがインストールされている必要があります。

## リモートコンテナの起動

.devcontainerが配置されたディレクトリをVSCodeで読み込みます。
VSCodeの起動後、ダイアログの`Reopen in Container`ボタンを選択します。
明示的に、ディレクトリを指定する場合は、VSCodeのパレットから `Remote Containers: Open Folder in Container...`を選択します。

## active_record_gem.rbの実行

リモートコンテナの起動後、コンテナ上のターミナルから`ruby active_record_gem.rb`を実行します。
※ workspaceディレクトリは、.devcontainerのあるホストOSのディレクトリがマウントされます

```bash
root ➜ /workspace $ ruby active_record_gem.rb 
Fetching gem metadata from https://rubygems.org/.........
Resolving dependencies...
Using concurrent-ruby 1.1.7
Using i18n 1.8.5
Using minitest 5.14.2
Using thread_safe 0.3.6
Using tzinfo 1.2.7
Using zeitwerk 2.4.0
Using activesupport 6.0.3
Using activemodel 6.0.3
Using activerecord 6.0.3
Using bundler 2.1.4
Using pg 1.2.3
-- create_table(:posts, {:force=>true})
D, [2020-10-06T12:18:34.300365 #1003] DEBUG -- :    (12.4ms)  DROP TABLE IF EXISTS "posts"
D, [2020-10-06T12:18:34.304599 #1003] DEBUG -- :    (4.0ms)  CREATE TABLE "posts" ("id" bigserial primary key)
   -> 0.0294s
-- create_table(:comments, {:force=>true})
D, [2020-10-06T12:18:34.307272 #1003] DEBUG -- :    (2.1ms)  DROP TABLE IF EXISTS "comments"
D, [2020-10-06T12:18:34.312048 #1003] DEBUG -- :    (4.1ms)  CREATE TABLE "comments" ("id" bigserial primary key, "post_id" integer)
   -> 0.0073s
D, [2020-10-06T12:18:34.346446 #1003] DEBUG -- :   ActiveRecord::InternalMetadata Load (0.9ms)  SELECT "ar_internal_metadata".* FROM "ar_internal_metadata" WHERE "ar_internal_metadata"."key" = $1 LIMIT $2  [["key", "environment"], ["LIMIT", 1]]
Run options: --seed 7737

# Running:

D, [2020-10-06T12:18:34.368941 #1003] DEBUG -- :    (0.4ms)  BEGIN
D, [2020-10-06T12:18:34.369828 #1003] DEBUG -- :   Post Create (0.7ms)  INSERT INTO "posts" DEFAULT VALUES RETURNING "id"
D, [2020-10-06T12:18:34.371368 #1003] DEBUG -- :    (1.2ms)  COMMIT
D, [2020-10-06T12:18:34.385669 #1003] DEBUG -- :    (0.4ms)  BEGIN
D, [2020-10-06T12:18:34.386396 #1003] DEBUG -- :   Comment Create (0.5ms)  INSERT INTO "comments" DEFAULT VALUES RETURNING "id"
D, [2020-10-06T12:18:34.387682 #1003] DEBUG -- :    (1.0ms)  COMMIT
D, [2020-10-06T12:18:34.391599 #1003] DEBUG -- :    (0.3ms)  BEGIN
D, [2020-10-06T12:18:34.392244 #1003] DEBUG -- :   Comment Update (0.5ms)  UPDATE "comments" SET "post_id" = $1 WHERE "comments"."id" = $2  [["post_id", 1], ["id", 1]]
D, [2020-10-06T12:18:34.393219 #1003] DEBUG -- :    (0.8ms)  COMMIT
D, [2020-10-06T12:18:34.394449 #1003] DEBUG -- :    (0.6ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."post_id" = $1  [["post_id", 1]]
D, [2020-10-06T12:18:34.396712 #1003] DEBUG -- :    (0.8ms)  SELECT COUNT(*) FROM "comments"
D, [2020-10-06T12:18:34.399609 #1003] DEBUG -- :   Comment Load (0.5ms)  SELECT "comments".* FROM "comments" ORDER BY "comments"."id" ASC LIMIT $1  [["LIMIT", 1]]
D, [2020-10-06T12:18:34.402319 #1003] DEBUG -- :   Post Load (0.4ms)  SELECT "posts".* FROM "posts" WHERE "posts"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
.

Finished in 0.041041s, 24.3659 runs/s, 73.0976 assertions/s.

1 runs, 3 assertions, 0 failures, 0 errors, 0 skips
```

## PostgreSQLとSQLクライアントの接続

PostgreSQL用のリモートコンテナを起動した場合、PostgreSQLが5000番ポートでローカルホストに公開されます。
SQLクライアントで以下の情報を入力し、接続することができます。

```
Host: localhost
Database: postgres
ユーザー名: postgres
パスワード: postgres
```

また、VSCodeの拡張機能の[SQL Server (mssql)](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)を使って接続することもできます。
