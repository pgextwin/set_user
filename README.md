# set_user — Windows x64

[日本語](README_ja.md) | English

pgextwin distributes **unofficial Windows x64 binaries** of [set_user](https://github.com/pgaudit/set_user). set_user enables controlled role switching with audit safeguards.

## Downloads and compatibility

Get the [latest published Release](https://github.com/pgextwin/set_user/releases/latest), or [browse all Releases](https://github.com/pgextwin/set_user/releases). Published PostgreSQL **14–18** packages appear in the [pgextwin catalog](https://pgextwin.github.io/website/en/). Select the ZIP matching your PostgreSQL major. PostgreSQL 14 community support ends November 12, 2026.

## Installation

1. Back up the relevant installation and review the [upstream setup instructions](https://github.com/pgaudit/set_user).
2. Stop PostgreSQL before replacing files. Extract the matching ZIP and place the files from lib/ and share/extension/ into the corresponding PostgreSQL directories.
3. Configure set_user in shared_preload_libraries and restart PostgreSQL. Check the upstream privilege and audit requirements before granting use of the extension.
4. Start PostgreSQL after configuring the package and run CREATE EXTENSION set_user; in each database that needs it.

Use the Release's SHA256SUMS.txt to verify downloads. SPDX SBOMs, vulnerability reports and signed attestations may be supplied with the Release. Test in a non-production environment first.

## License

For SQL/API usage and limitations, consult [upstream set_user](https://github.com/pgaudit/set_user). pgextwin is independent from the upstream authors. The included license notices apply.
