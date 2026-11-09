# set_user — Windows x64 バイナリ

[English](README.md) | 日本語

[set_user](https://github.com/pgaudit/set_user)の**非公式Windows x64バイナリ**を配布しています。権限を管理しながらデータベースロールを切り替えるための拡張機能です。

## ダウンロードと対応環境

[最新の公開Release](https://github.com/pgextwin/set_user/releases/latest)から、利用中のPostgreSQLメジャーバージョンと一致するZIPを選んでください。[過去のReleases](https://github.com/pgextwin/set_user/releases)も確認できます。PostgreSQL **14〜18**向けの配布状況は[pgextwinカタログ](https://pgextwin.github.io/website/ja/)を参照してください。PostgreSQL 14のコミュニティサポートは2026年11月12日に終了します。

## インストール

1. バックアップを取得し、[上流の導入手順](https://github.com/pgaudit/set_user)を確認します。
2. 既存ファイルを置き換える前にPostgreSQLを停止します。対応するZIP内のlib/とshare/extension/以下を、PostgreSQLの同名ディレクトリに配置します。
3. `postgresql.conf`の`shared_preload_libraries`にset_userを追加します。利用権限や監査の要件は上流ドキュメントで確認してください。
4. 設定を反映してPostgreSQLを起動し、利用するデータベースで`CREATE EXTENSION set_user;`を実行します。稼働中のPostgreSQLで事前ロード設定を変更した場合は、再起動が必要です。

ダウンロードしたZIPを、Releaseに掲載された`SHA256SUMS.txt`と照合してください。SPDX形式のSBOM、脆弱性レポート、署名付き証明の有無はReleaseで確認できます。本番環境に導入する前に、検証環境で動作を確認してください。

## ライセンス

利用方法や制限事項は[上流のset_user](https://github.com/pgaudit/set_user)を参照してください。本配布は上流公式のWindows版ではありません。ライセンスは同梱の表記に従います。
