# set_user Windows binaries

[日本語](README_ja.md) | **English**

This repository provides **unofficial Windows x64 binaries** of [set_user](https://github.com/pgaudit/set_user).

Upstream source:

- repository: `pgaudit/set_user`
- ref: `REL4_2_0`
- version: 4.2.0
- license: PostgreSQL License

The package is validated on PostgreSQL 14, 15, 16, 17, and 18.

## Download

The first pgextwin release is planned as:

~~~text
v4.2.0-windows.1
~~~

Assets use:

~~~text
set_user-REL4_2_0-pg14-windows-x64.zip
...
set_user-REL4_2_0-pg18-windows-x64.zip
~~~

Each package contains:

~~~text
lib/
  set_user.dll
share/
  extension/
    set_user.control
    set_user--*.sql
include/
  set_user.h
LICENSE
UPSTREAM-README.md
UPSTREAM-CHANGELOG.md
PACKAGE-INFO.txt
~~~

The public `set_user.h` header is included because upstream installs it for extensions that register set_user post-execution hooks.

## Installation

1. Select the ZIP matching the PostgreSQL major version.
2. Stop PostgreSQL before replacing the DLL.
3. Copy `lib/set_user.dll` to PostgreSQL's `lib`.
4. Copy `share/extension/*` to PostgreSQL's `share/extension`.
5. Copy `include/set_user.h` to PostgreSQL's `include`.
6. Add `set_user` to `shared_preload_libraries`.
7. Restart PostgreSQL.
8. Run:

~~~sql
CREATE EXTENSION set_user;
~~~

See [docs/windows_ja.md](docs/windows_ja.md) and the upstream README before deploying it as a privilege-management control.

## Windows compatibility

Upstream 4.2.0 declares:

~~~c
extern Datum set_user(PG_FUNCTION_ARGS);
~~~

before `PG_FUNCTION_INFO_V1(set_user)`.

On PostgreSQL 16 and newer, `PG_FUNCTION_INFO_V1()` declares the SQL-callable function with `PGDLLEXPORT` on Windows. The earlier non-export declaration therefore creates an MSVC linkage conflict.

The pgextwin build applies a minimal build-workspace compatibility edit to use `PGDLLEXPORT` for that declaration. The pinned upstream repository and LICENSE remain the source of truth.

The DLL export definition includes module entry points and SQL-callable functions discovered from the source.

## Functional CI

Every supported PostgreSQL major must pass:

1. exact upstream LICENSE verification,
2. MSVC x64 build,
3. startup with `shared_preload_libraries=set_user`,
4. `CREATE EXTENSION set_user`,
5. create a non-superuser probe role,
6. call `set_user()` and verify `current_user` changes while `session_user` remains postgres,
7. call `reset_user()` and verify the original user is restored,
8. verify both transition records in the actual PostgreSQL server log,
9. package DLL, SQL/control files, and `set_user.h`.

Pull requests and `main` validate only. A `release/<tag>` branch publishes only after the full matrix passes.

## License

LICENSE is copied from upstream set_user and verified against the pinned source checkout.

These binaries are unofficial pgextwin builds.
