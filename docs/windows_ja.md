# set_user Windows x64 バイナリ利用ガイド

## ファイル配置

PostgreSQLを停止してから、対象majorのZIPを展開します。

~~~text
ZIP\lib\set_user.dll
  -> <PostgreSQL>\lib\set_user.dll

ZIP\share\extension\*
  -> <PostgreSQL>\share\extension\

ZIP\include\set_user.h
  -> <PostgreSQL>\include\set_user.h
~~~

## preload

`postgresql.conf`:

~~~conf
shared_preload_libraries = 'set_user'
~~~

既に他Extensionをpreloadしている場合はカンマ区切りで追加します。

PostgreSQLを再起動後:

~~~sql
CREATE EXTENSION set_user;
~~~

## 基本的な確認

権限を付与した管理者が、許可されたroleへ遷移する例:

~~~sql
SELECT set_user('target_role');
SELECT current_user, session_user;
SELECT reset_user();
SELECT current_user, session_user;
~~~

実際の権限設計ではupstream READMEにあるallowlist/blocklist、superuser transition、session authorization、ログ動作を確認してください。

## Windows compatibility

upstream 4.2.0には、PG16+のWindows headerが生成する `PGDLLEXPORT` 宣言と競合するprototypeがあります。pgextwin packageはbuild時にその宣言だけをWindows用linkageへ合わせています。

PG14/15も同じbuild hookを通しますが、CIで各majorの実動作を個別に確認します。

## CIで確認する内容

- PostgreSQL 14〜18
- `shared_preload_libraries=set_user`
- `CREATE EXTENSION set_user`
- 非superuser roleへのtransition
- `session_user`保持
- `reset_user()`
- transition server log
- headerを含むpackage構成

本体仕様とセキュリティ判断はupstream set_userのドキュメントを優先してください。
