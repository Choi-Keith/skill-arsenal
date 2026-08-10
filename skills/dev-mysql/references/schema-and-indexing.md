# Schema Defaults and Indexing

## Schema Defaults

```sql
CREATE TABLE orders (
    id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    account_id BIGINT UNSIGNED NOT NULL,
    status VARCHAR(32) NOT NULL,
    total DECIMAL(15, 2) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deleted_at DATETIME NULL,
    PRIMARY KEY (id),
    KEY idx_orders_account_status_created (account_id, status, created_at),
    KEY idx_orders_active (account_id, deleted_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

Default choices:

| Use Case | Prefer | Avoid |
| --- | --- | --- |
| Surrogate primary keys | `BIGINT UNSIGNED AUTO_INCREMENT` | `INT` for tables that can grow beyond 2B rows |
| UUID lookup keys | `BINARY(16)` with conversion helpers | `VARCHAR(36)` primary keys on hot tables |
| Money and exact quantities | `DECIMAL(p, s)` | `FLOAT` or `DOUBLE` |
| User-facing text | `utf8mb4` tables and indexes | MySQL `utf8` / `utf8mb3` defaults |
| Application timestamps | `DATETIME` with UTC managed by the app | Assuming `DATETIME` stores time zone metadata |
| Soft deletes | `deleted_at DATETIME NULL` plus scoped indexes | Filtering soft-deleted rows without an index |
| Extensible status values | lookup table or constrained `VARCHAR` | `ENUM` when values change often |

## Indexing

Composite index order usually follows equality predicates first, then range or
sort columns:

```sql
CREATE INDEX idx_orders_account_status_created
    ON orders (account_id, status, created_at);

SELECT id, total
FROM orders
WHERE account_id = ?
  AND status = 'pending'
  AND created_at >= ?
ORDER BY created_at DESC
LIMIT 50;
```

Use `EXPLAIN` before adding or changing an index:

```sql
EXPLAIN
SELECT id, total
FROM orders
WHERE account_id = 123 AND status = 'pending'
ORDER BY created_at DESC
LIMIT 50;
```

Signals to investigate:

| Field | Risk Signal |
| --- | --- |
| `type` | `ALL` on a large table |
| `key` | `NULL` when a selective predicate exists |
| `rows` | Very high row estimate for an interactive path |
| `Extra` | `Using temporary`, `Using filesort`, or broad `Using where` |

Avoid adding indexes blindly. Each index increases write cost, migration time,
backup size, and buffer-pool pressure.
