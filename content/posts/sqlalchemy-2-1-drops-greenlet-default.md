---
title: "SQLAlchemy 2.1 drops default greenlet, requires Python 3.11"
slug: "sqlalchemy-2-1-drops-greenlet-default"
date: 2026-10-12T05:04:16+0000
summary: "SQLAlchemy 2.1.0 is now what `pip install sqlalchemy` installs. The upgrade is meant to be easier than 2.0 was, but it has a few hard changes: greenlet is no longer installed by default (use `sqlalchemy[asyncio]` for async), Python 3.11 is the minimum, and bare `postgresql://` and `oracle://` URLs now pick psycopg 3 and oracledb."
source: "https://www.sqlalchemy.org/blog/2026/09/24/sqlalchemy-2.1.0-released/"
source_title: "SQLAlchemy 2.1.0 Released"
source_author: "Mike Bayer"
source_site: "sqlalchemy.org"
source_date: "2026-09-24"
hn_url: "https://news.ycombinator.com/item?id=50043030"
---
SQLAlchemy 2.1.0, the first production release of the 2.1 series, is now the default `pip install sqlalchemy` version. The maintainer says it is much less dramatic than 2.0 and upgrades shouldn't be hard, but some changes may need workflow adjustments.

**Breaking changes**
- The greenlet dependency, used by all asyncio features, no longer installs by default. Use `pip install sqlalchemy[asyncio]`.
- URLs of the form `postgresql://` and `oracle://` with no explicit driver now select psycopg (psycopg3) and oracledb.
- Python 3.11 is the minimum. The author says supporting EOL Pythons has become harder as pip, nox and GitHub Actions drop old versions quickly.

**New features**
- `tstring()`, a variant of `text()` that takes Python T-strings and turns interpolations into bind parameters.
- `CreateView` and `CREATE TABLE AS` support, long a documented recipe.
- Support for Microsoft's `mssql-python` driver, built with the Microsoft team.

The release comes three years after 2.0. The post points to the "What's New in SQLAlchemy 2.1?" migration guide and the changelog for the full list.
