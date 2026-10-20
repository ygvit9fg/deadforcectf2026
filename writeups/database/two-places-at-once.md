# Two Places at Once

- **Category:** Database
- **Points:** 450

## Objective

Identify the account whose access logs show activity from at least two distinct workstations at the same timestamp.

## Investigation

I started by inspecting the available databases and tables. The first `SHOW TABLES;` attempt returned `No database selected`, so I listed the databases, selected `aurora_audit`, and inspected `access_log` and `users`.

```sql
SHOW DATABASES;
USE aurora_audit;
SHOW TABLES;
DESCRIBE access_log;
DESCRIBE users;
```

I joined the access log to the user table by `user_id`, grouped events by username and timestamp, and retained groups with two or more distinct workstation values:

```sql
SELECT
    u.username,
    a.accessed_at,
    COUNT(DISTINCT a.workstation) AS workstation_count
FROM access_log a
JOIN users u ON a.user_id = u.user_id
GROUP BY u.username, a.accessed_at
HAVING COUNT(DISTINCT a.workstation) >= 2
ORDER BY workstation_count DESC;
```

The query returned eight matching rows, all associated with `kbhatt`.

## Flag

```text
deadface{kbhatt}
```

## Takeaway

The important part was counting **distinct** workstations for each user and timestamp, rather than counting every log row. `HAVING` filters the grouped results after aggregation.
