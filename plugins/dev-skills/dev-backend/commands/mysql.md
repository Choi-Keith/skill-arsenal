---
description: MySQL/MariaDB patterns — schema design, indexing, query patterns, transactions, connection pools, replication, security, and production configuration
argument-hint: "<task, schema, or query>"
---

# /mysql -- MySQL Patterns Guide

Apply the `dev-mysql` skill to: $ARGUMENTS

Start with the engine/version check (`SELECT VERSION()`), then load the matching topic file from the skill's `references/`:

- `schema-and-indexing.md` — table design, column types, composite indexes, `EXPLAIN` review
- `query-patterns.md` — upserts, keyset pagination, JSON columns, full-text search
- `transactions.md` — locking order, deadlocks, `SKIP LOCKED` queues
- `connection-pools.md` — SQLAlchemy/mysql2 pool configuration
- `operations.md` — slow log, `PROCESSLIST`, replicas, `my.cnf`
- `security.md` — users, grants, TLS, least privilege
