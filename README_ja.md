# set_user Windows バイナリ

[English](README.md) | **日本語**

このリポジトリでは、[set_user](https://github.com/pgaudit/set_user) 4.2.0 の **非公式 Windows x64 バイナリ**を提供します。

使用するupstreamは `pgaudit/set_user` の `REL4_2_0` です。PostgreSQL 14〜18を対象に検証します。

公開済みRelease tag:

~~~text
v4.2.0-windows.1
~~~

## 導入

1. PostgreSQLメジャーに一致するZIPを選択します。
2. PostgreSQLを停止します。
3. `lib/set_user.dll` を `lib` へコピーします。
4. `share/extension/*` を `share/extension` へコピーします。
5. `include/set_user.h` をPostgreSQLの `include` へコピーします。
6. `shared_preload_libraries` に `set_user` を追加します。
7. PostgreSQLを再起動します。
8. `CREATE EXTENSION set_user;` を実行します。

詳細は [docs/windows_ja.md](docs/windows_ja.md) とupstream READMEを参照してください。

## Windows互換処理

PostgreSQL 16以降では `PG_FUNCTION_INFO_V1(set_user)` がWindows向け `PGDLLEXPORT` 宣言を生成しますが、upstream 4.2.0はその前にexport指定なしの `extern Datum set_user(...)` を宣言しています。

pgextwinではbuild workspace内だけで、このprototypeを `PGDLLEXPORT` と一致させる最小compatibility editを適用します。upstream repositoryをforkしたsource copyは保持しません。

## CIの合格条件

PG14〜18で実際に:

- preloadしてPostgreSQLを起動
- `CREATE EXTENSION set_user`
- postgresから非superuser roleへ `set_user()`
- `current_user` / `session_user` を検証
- `reset_user()` でpostgresへ復帰
- PostgreSQL server logの両方向transition記録を確認

まで行います。

set_userは権限管理に関わるExtensionなので、実運用ではupstreamのallowlist/blocklist、superuser監査、設定パラメータを必ず確認してください。

本バイナリはpgextwinによる非公式配布です。
