# Advanced Oracle Database Interview Questions — Senior Developer / DBA

---

## 1. Oracle Architecture & Instance Internals

- What is an Oracle Database instance? How does it differ from a database?
- What are the components of an Oracle instance? (SGA + background processes)
- What is the System Global Area (SGA)? What are its components?
  - Buffer Cache, Shared Pool (Library Cache + Data Dictionary Cache), Redo Log Buffer, Large Pool, Java Pool, Streams Pool
- What is the Program Global Area (PGA)? How does it differ from SGA?
- What is the Database Buffer Cache? What is its purpose?
- What are the buffer states: free, pinned, dirty, clean?
- What is the LRU (Least Recently Used) algorithm in the buffer cache?
- What is the Shared Pool? What is the Library Cache? What is the Data Dictionary Cache?
- What is a soft parse vs hard parse? Why are hard parses expensive?
- What is the Redo Log Buffer? How does LGWR flush it?
- What is the Large Pool? What uses it? (RMAN, parallel query, shared server)
- What are Oracle background processes: DBWR, LGWR, CKPT, SMON, PMON, ARCn, RECO, MMON, MMNL?
- What does DBWR do? When does it write dirty buffers to disk?
- What does LGWR do? What triggers a log write? (commit, 1/3 full, 3 seconds, DBWR request)
- What does CKPT do? What is a checkpoint?
- What does SMON do? (Instance recovery, coalescing free space)
- What does PMON do? (Process cleanup, dead session cleanup, service registration)
- What does ARCn do? (Archive redo logs for recovery)
- What is the difference between Oracle 12c multitenant (CDB/PDB) and non-CDB architecture?
- What is a CDB (Container Database)? What is a PDB (Pluggable Database)?
- What is the root container (`CDB$ROOT`)? What is `PDB$SEED`?
- What is the Oracle memory management modes: Manual, ASMM (AMM disabled), AMM (Automatic Memory Management)?
- What is `MEMORY_TARGET` vs `SGA_TARGET` vs `SGA_MAX_SIZE`?
- What is the Oracle process model: dedicated server vs shared server (MTS)?
- What is a listener? What is `tnsnames.ora`? What is `sqlnet.ora`?
- What is Oracle Net (SQL*Net)?

> **Best Practices:**
> - Set `SGA_TARGET` and `PGA_AGGREGATE_TARGET` explicitly rather than relying on `MEMORY_TARGET` — gives finer control.
> - Monitor hard parse rates — high hard parses indicate missing bind variables or non-reusable SQL.
> - Size the Buffer Cache to reduce physical I/O — monitor `V$DB_CACHE_ADVICE`.
> - Use bind variables in all OLTP SQL — prevents hard parses and Shared Pool fragmentation.
> - Monitor `V$SGASTAT` and `V$PGASTAT` regularly for memory pressure.

---

## 2. Oracle Storage Architecture

- What is a tablespace? What types exist? (SYSTEM, SYSAUX, TEMP, UNDO, user-defined)
- What is a datafile? How does a tablespace relate to datafiles?
- What is an extent? What is a segment? What is a block?
- What is the Oracle block size? What is `DB_BLOCK_SIZE`? (Default 8KB)
- What is a segment? What types of segments exist? (Table, Index, Undo, Temp, LOB, Bootstrap)
- What is extent management: dictionary-managed vs locally managed tablespaces?
- What is segment space management: manual vs automatic (ASSM — Automatic Segment Space Management)?
- What is a High Water Mark (HWM)? What is the difference between HWM and actual data?
- What is HWM impact on full table scans?
- What is `TRUNCATE` vs `DELETE` for HWM?
- What is a temporary tablespace? What operations use it?
- What is the UNDO tablespace? How is it different from rollback segments?
- What is `UNDO_RETENTION`? What is guaranteed undo retention?
- What is Automatic Storage Management (ASM)?
- What is an ASM disk group? What are redundancy levels? (External, Normal, High)
- What is an ASM failure group?
- What is Oracle Managed Files (OMF)?
- What is a bigfile tablespace vs smallfile tablespace?
- What is a redo log group? What is a redo log member?
- What is online redo log vs archived redo log?
- What is log switch? What is `ALTER SYSTEM SWITCH LOGFILE`?
- What is multiplexing redo logs? Why is it important?
- What is the control file? What does it contain?

> **Best Practices:**
> - Use Locally Managed Tablespaces (LMT) with ASSM — reduces contention and fragmentation.
> - Multiplex control files across different disks/disk groups.
> - Multiplex redo log members — losing all members of a group causes data loss.
> - Set `UNDO_RETENTION` higher than your longest transaction for read consistency.
> - Monitor HWM on frequently deleted tables — consider `MOVE` or `SHRINK` to reclaim space.

---

## 3. SQL Fundamentals & Oracle-Specific SQL

- What are the SQL categories: DML, DDL, DCL, TCL, DQL?
- What is `DUAL`? Why does Oracle have it?
- What is `ROWNUM`? How does it differ from `ROW_NUMBER()`?
- What is `ROWID`? What information does it encode? (Object, file, block, row)
- What is the difference between `WHERE` and `HAVING`?
- What is the SQL execution order? (FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT)
- What is `CONNECT BY`? What is hierarchical query?
- What is `START WITH` in a hierarchical query?
- What is `LEVEL` pseudocolumn? What is `SYS_CONNECT_BY_PATH`?
- What is `NOCYCLE` in hierarchical queries?
- What is `PRIOR` keyword?
- What is `CONNECT_BY_ISLEAF`, `CONNECT_BY_ISCYCLE`?
- What is the `MERGE` statement (UPSERT)?
- What is `INSERT ALL` vs `INSERT FIRST`?
- What is multi-table insert?
- What is `RETURNING INTO` clause?
- What is `FOR UPDATE` clause? What is `NOWAIT`? `SKIP LOCKED`?
- What is `BULK COLLECT`? What is `FORALL`?
- What is `WITH` clause (CTE — Common Table Expression)?
- What is a recursive CTE in Oracle 11g R2+?
- What is the `CASE` expression vs `DECODE` function?
- What is `NVL`, `NVL2`, `NULLIF`, `COALESCE`?
- What is `LNNVL`?
- What is `GREATEST`, `LEAST`?
- What is `PIVOT` and `UNPIVOT`?

> **Best Practices:**
> - Always use bind variables (`:var`) instead of literal values — critical for Shared Pool efficiency.
> - Prefer `COALESCE` over `NVL` — COALESCE is ANSI standard and evaluates lazily.
> - Avoid `SELECT *` in production code — breaks when column order or set changes.
> - Use `MERGE` for upsert operations — single statement, atomic, better than INSERT + UPDATE.
> - Use `RETURNING INTO` to capture generated IDs after DML — avoids extra SELECT.

---

## 4. Oracle Data Types

- What are Oracle numeric types: `NUMBER(p,s)`, `BINARY_FLOAT`, `BINARY_DOUBLE`, `INTEGER`, `PLS_INTEGER`?
- What is `NUMBER(10,2)`? What is precision and scale?
- What is the difference between `NUMBER` and `BINARY_FLOAT`/`BINARY_DOUBLE`?
- What are Oracle character types: `VARCHAR2`, `NVARCHAR2`, `CHAR`, `NCHAR`, `CLOB`, `NCLOB`?
- What is the maximum size of `VARCHAR2` in SQL vs PL/SQL? (4000 bytes SQL / 32767 bytes PL/SQL; 32767 in extended string mode)
- What is `CHAR` vs `VARCHAR2`? (CHAR is fixed-length, padded with spaces)
- What are Oracle date/time types: `DATE`, `TIMESTAMP`, `TIMESTAMP WITH TIME ZONE`, `TIMESTAMP WITH LOCAL TIME ZONE`, `INTERVAL`?
- What is the Oracle `DATE` type? Does it include time? (Yes — stores up to seconds)
- What is the difference between `DATE` and `TIMESTAMP`?
- What is `INTERVAL YEAR TO MONTH` vs `INTERVAL DAY TO SECOND`?
- What is `SYSDATE` vs `SYSTIMESTAMP` vs `CURRENT_DATE` vs `CURRENT_TIMESTAMP`?
- What is `RAW` and `LONG RAW`? Are they still recommended? (No — use `BLOB`)
- What are Oracle LOB types: `BLOB`, `CLOB`, `NCLOB`, `BFILE`?
- What is `BFILE`? Where does it store data? (External file — stores only a pointer)
- What is `XMLTYPE`?
- What is `SDO_GEOMETRY` (Oracle Spatial)?
- What is `ROWID` type?
- What is `UROWID`?
- What is `BOOLEAN` in SQL vs PL/SQL? (Not available in SQL until Oracle 23c; available in PL/SQL)
- What is `JSON` type (Oracle 21c+)?

> **Best Practices:**
> - Use `NUMBER` for financial data — exact decimal arithmetic, no floating-point errors.
> - Use `VARCHAR2` over `CHAR` — CHAR wastes space and causes comparison issues.
> - Use `TIMESTAMP WITH TIME ZONE` for audit columns — preserves the full time context.
> - Store dates as `DATE` or `TIMESTAMP` — never as `VARCHAR2`.
> - Use `CLOB` instead of `LONG` for large text — `LONG` is deprecated.

---

## 5. Indexes

- What is a B-tree index? How does it work?
- What is the structure of a B-tree index in Oracle? (Root → Branch → Leaf blocks)
- What is a unique index vs non-unique index?
- What is a composite (concatenated) index? What is the leading column rule?
- What is a function-based index (FBI)?
- What is a bitmap index? When should you use it? (Low cardinality, read-heavy, DW)
- What is a bitmap join index?
- What are the downsides of bitmap indexes for DML? (Row-level locking becomes segment-level locking)
- What is an index-organized table (IOT)?
- What is a cluster index? What is a hash cluster?
- What is a reverse key index? When is it useful? (Monotonically increasing keys — RAC contention)
- What is a descending index?
- What is an invisible index? What is `SKIP_UNUSABLE_INDEXES`?
- When does Oracle NOT use an index?
  - Function on indexed column (unless FBI), implicit type conversion, leading `%` LIKE, `IS NULL`, `!=`, `NOT IN`, full table scan is cheaper
- What is index selectivity? What is cardinality?
- What is an index range scan vs full index scan vs fast full index scan vs index skip scan?
- What is the difference between index range scan and index full scan?
- What is index skip scan? When does it occur?
- What is `ANALYZE` vs `DBMS_STATS`? Which should you use? (`DBMS_STATS`)
- What is column histogram? When does Oracle create one?
- What is `MONITORING USAGE` on an index?
- What is `V$OBJECT_USAGE`?
- What is index coalescing vs rebuilding? When is each appropriate?
- What is a global index vs local index on a partitioned table?
- What is an unusable index? When does it become unusable?

> **Best Practices:**
> - Index foreign key columns — prevents full table lock during parent DELETE (lock escalation).
> - Use function-based indexes instead of disabling column functions in WHERE clauses.
> - Gather statistics with `DBMS_STATS` regularly — stale statistics cause bad execution plans.
> - Avoid over-indexing OLTP tables — every index slows DML.
> - Use invisible indexes to test impact before making an index available to the optimizer.
> - Monitor index usage with `V$OBJECT_USAGE` before dropping — unused indexes waste storage and DML overhead.

---

## 6. Query Optimization & Execution Plans

- What is the Oracle Cost-Based Optimizer (CBO)?
- What is the Rule-Based Optimizer (RBO)? Is it still available? (Desupported in 10g)
- What is an execution plan? How do you view it?
  - `EXPLAIN PLAN FOR ...` + `SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY)`
  - `SET AUTOTRACE ON`
  - `V$SQL_PLAN`, `DBMS_XPLAN.DISPLAY_CURSOR`
- What is the difference between `EXPLAIN PLAN` and actual runtime plan?
- What is `DBMS_XPLAN.DISPLAY_CURSOR`? What does `format => 'ALLSTATS LAST'` show?
- What are the plan operations: TABLE ACCESS FULL, TABLE ACCESS BY INDEX ROWID, INDEX RANGE SCAN, NESTED LOOPS, HASH JOIN, SORT MERGE JOIN, SORT AGGREGATE, etc.?
- What is a Full Table Scan? When is it better than an index scan?
- What is a Nested Loop join? When is it optimal? (Small outer loop, indexed inner table)
- What is a Hash Join? When is it used? (Large tables, no usable index)
- What is a Sort Merge Join? When is it used? (Pre-sorted inputs, range conditions)
- What is cardinality estimate? What is statistics staleness?
- What is a bind variable peeking? What is Adaptive Cursor Sharing (Oracle 11g+)?
- What is SQL Plan Management (SPM)? What are SQL plan baselines?
- What is a SQL Profile? What is a SQL Patch?
- What are optimizer hints? Give examples: `FULL`, `INDEX`, `USE_NL`, `USE_HASH`, `LEADING`, `NO_MERGE`, `PARALLEL`, `APPEND`
- What is the `/*+ APPEND */` hint? What is a direct path insert?
- What is cardinality feedback (Oracle 11g+)?
- What is Adaptive Query Optimization (Oracle 12c)?
- What is adaptive plans? What is adaptive statistics?
- What is dynamic statistics (formerly dynamic sampling)?
- What is the `FIRST_ROWS(n)` optimizer mode vs `ALL_ROWS`?
- What is `CURSOR_SHARING`? What are the values: `EXACT`, `FORCE`, `SIMILAR`?
- What is a cursor? What is a shared cursor vs private cursor?
- What is `V$SQL`, `V$SQLAREA`, `V$SQL_PLAN`?
- What is AWR (Automatic Workload Repository)? What is ASH (Active Session History)?
- What is ADDM (Automatic Database Diagnostic Monitor)?
- What is a SQL Tuning Advisor?

> **Best Practices:**
> - Use `DBMS_XPLAN.DISPLAY_CURSOR` with `ALLSTATS LAST` to see actual vs estimated cardinality.
> - Large cardinality estimate errors indicate stale or missing statistics — run `DBMS_STATS`.
> - Use hints sparingly — they are fragile and prevent the optimizer from adapting to data changes.
> - Use SQL Plan Baselines (SPM) to stabilize execution plans in production.
> - Review AWR Top SQL reports regularly — focus on Elapsed Time per execution, not total.
> - Always test query performance with representative data volumes and statistics.

---

## 7. Joins & Subqueries

- What are the Oracle join types: INNER JOIN, LEFT/RIGHT/FULL OUTER JOIN, CROSS JOIN, NATURAL JOIN?
- What is Oracle's old join syntax (`+` operator)? How does it map to ANSI syntax?
- What is a self-join? Give an example.
- What is a semi-join? What is an anti-join?
- What is `EXISTS` vs `IN`? Which is better for correlated subqueries?
- What is a correlated subquery? When does it perform poorly?
- What is the difference between `NOT IN` and `NOT EXISTS`? Why is `NOT IN` dangerous with NULLs?
- What is a scalar subquery? Where can it appear?
- What is subquery unnesting? (Optimizer transforms subquery to join)
- What is a lateral join (`LATERAL` / `CROSS APPLY`)? (Oracle 12c)
- What is `OUTER APPLY`?
- What is a `WITH` clause (CTE)? How does Oracle materialize it?
- What is `WITH ... AS (MATERIALIZE)` hint?
- What is the difference between `UNION`, `UNION ALL`, `INTERSECT`, `MINUS`?
- What is `LISTAGG`? What is `LISTAGG(... ON OVERFLOW TRUNCATE)`? (Oracle 19c)
- What is `XMLAGG` for string aggregation?
- What is `WM_CONCAT` (deprecated)?

> **Best Practices:**
> - Prefer `EXISTS` over `IN` for correlated subqueries — stops at first match.
> - Avoid `NOT IN` when the subquery can return NULLs — use `NOT EXISTS` instead.
> - Use CTEs (`WITH` clause) for readability, but be aware Oracle may or may not materialize them.
> - Use `UNION ALL` over `UNION` when duplicates are not a concern — avoids sort/dedup overhead.

---

## 8. Analytic (Window) Functions

- What are analytic functions? How do they differ from aggregate functions?
- What is the `OVER()` clause?
- What is `PARTITION BY` in analytic functions?
- What is `ORDER BY` in analytic functions?
- What is the window frame? What are `ROWS` vs `RANGE`?
- What is `UNBOUNDED PRECEDING`, `CURRENT ROW`, `UNBOUNDED FOLLOWING`?
- What are ranking functions: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `NTILE(n)`?
- What is the difference between `RANK()` and `DENSE_RANK()`?
- What is `ROW_NUMBER()` vs `RANK()` for pagination?
- What are navigation functions: `LAG()`, `LEAD()`, `FIRST_VALUE()`, `LAST_VALUE()`, `NTH_VALUE()`?
- What is `LAST_VALUE` default frame issue? (Defaults to `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` — not the full partition)
- What are aggregate analytic functions: `SUM() OVER()`, `AVG() OVER()`, `COUNT() OVER()`, `MAX() OVER()`, `MIN() OVER()`?
- What is a running total? How do you implement it?
- What is a moving average? How do you implement it?
- What is `RATIO_TO_REPORT`?
- What is `PERCENT_RANK()` vs `CUME_DIST()`?
- What is `LISTAGG() WITHIN GROUP (ORDER BY)`?
- What is `MEDIAN`? `PERCENTILE_CONT`? `PERCENTILE_DISC`?
- What is `KEEP (DENSE_RANK FIRST/LAST ORDER BY)`?

> **Best Practices:**
> - Use `ROW_NUMBER() OVER (PARTITION BY ... ORDER BY ...)` for deduplication and pagination.
> - Specify explicit window frame for `LAST_VALUE` — default frame gives unexpected results.
> - Analytic functions execute after `WHERE`/`GROUP BY`/`HAVING` — use a subquery/CTE if filtering on analytic result.
> - Use `RANK()` for competition ranking (gaps allowed); `DENSE_RANK()` when gaps are undesirable.

---

## 9. PL/SQL Fundamentals

- What is PL/SQL? How does it differ from SQL?
- What is a PL/SQL block structure? (DECLARE → BEGIN → EXCEPTION → END)
- What are PL/SQL variable types: scalar, composite (`%TYPE`, `%ROWTYPE`), reference, LOB?
- What is `%TYPE`? What is `%ROWTYPE`?
- What is a record type in PL/SQL?
- What is a PL/SQL collection: `VARRAY`, `Nested Table`, `Associative Array (Index-by Table)`?
- What is the difference between VARRAY, Nested Table, and Associative Array?
- What is `BULK COLLECT INTO`? What is the `LIMIT` clause?
- What is `FORALL`? How does it differ from a loop with individual DML?
- What is `SQL%ROWCOUNT`, `SQL%FOUND`, `SQL%NOTFOUND`, `SQL%ISOPEN`?
- What is `RETURNING INTO` in PL/SQL?
- What is a cursor? What is an implicit cursor vs explicit cursor?
- What is `CURSOR FOR LOOP`? What is its advantage? (Automatic open/fetch/close)
- What is `REF CURSOR`? What is `SYS_REFCURSOR`?
- What is a parameterized cursor?
- What is cursor attribute `%FOUND`, `%NOTFOUND`, `%ROWCOUNT`, `%ISOPEN`?
- What is `OPEN-FETCH-CLOSE` pattern vs `FOR LOOP`?
- What is exception handling in PL/SQL?
- What are predefined exceptions: `NO_DATA_FOUND`, `TOO_MANY_ROWS`, `DUP_VAL_ON_INDEX`, `VALUE_ERROR`, `ZERO_DIVIDE`, `CURSOR_ALREADY_OPEN`?
- What is `OTHERS` handler? What is `SQLCODE` and `SQLERRM`?
- What is `RAISE_APPLICATION_ERROR(error_number, message)`?
- What is user-defined exception?
- What is `EXCEPTION_INIT` pragma?
- What is `PRAGMA AUTONOMOUS_TRANSACTION`? When is it used?
- What is `PRAGMA SERIALLY_REUSABLE`?
- What is `PRAGMA INLINE` (Oracle 11g)?

> **Best Practices:**
> - Use `BULK COLLECT` + `FORALL` for set-based DML — 10x–100x faster than row-by-row processing.
> - Always set a `LIMIT` clause in `BULK COLLECT` for large datasets — prevents PGA overload.
> - Use `%TYPE` and `%ROWTYPE` — code remains valid after column type changes.
> - Never use bare `WHEN OTHERS THEN NULL` — silently swallows errors.
> - Log exceptions with `SQLERRM` and `DBMS_UTILITY.FORMAT_ERROR_BACKTRACE`.
> - Use `AUTONOMOUS_TRANSACTION` only for logging — not for main data flow.

---

## 10. Stored Procedures, Functions & Packages

- What is a stored procedure? What is a function? What is the key difference? (Function returns a value)
- What are `IN`, `OUT`, `IN OUT` parameters?
- What is `NOCOPY` hint for OUT parameters?
- What is a function deterministic? (`DETERMINISTIC` keyword — enables function-based indexing and caching)
- What is `PARALLEL_ENABLE`?
- What is `PIPELINED` table function? What does it enable? (Stream rows — lazy evaluation)
- What is a package? What are its components? (Spec + Body)
- What is a package spec vs package body?
- What is package state? What is a package-level variable? (Session-scoped state)
- What is package initialization section?
- What is overloading in PL/SQL? Where is it supported? (Package procedures/functions)
- What is a trigger? What are the trigger types?
  - Row-level vs statement-level (`FOR EACH ROW`)
  - BEFORE vs AFTER vs INSTEAD OF
  - DML triggers, DDL triggers, system triggers
- What is `:NEW` and `:OLD` in a row-level trigger?
- What is `INSTEAD OF` trigger? When is it used? (Views)
- What is a DDL trigger? What events can it capture?
- What is a system trigger (`ON DATABASE`, `ON SCHEMA`)?
- What is the trigger firing order when multiple triggers exist on the same event?
- What is `FOLLOWS` / `PRECEDES` clause in triggers (Oracle 11g)?
- What is a compound trigger (Oracle 11g)?
- What is a mutating table error (ORA-04091)? How do you resolve it?
- What is `DBMS_OUTPUT.PUT_LINE`?
- What is `UTL_FILE`? What does it enable?
- What is `DBMS_PIPE`?
- What is `DBMS_ALERT`?

> **Best Practices:**
> - Use packages to group related procedures/functions — better encapsulation, single compilation unit.
> - Avoid triggers for business logic — they are invisible, order-dependent, and hard to debug.
> - Use `NOCOPY` hint for large `OUT` or `IN OUT` parameters — avoids unnecessary copy.
> - Mark functions `DETERMINISTIC` when they always return the same result for the same inputs.
> - Use pipelined table functions to return large result sets lazily — avoids building full collection in PGA.

---

## 11. Transactions & Concurrency

- What is a transaction in Oracle?
- What is ACID? How does Oracle implement each property?
- What is `COMMIT`? What is `ROLLBACK`? What is `SAVEPOINT`?
- What is `ROLLBACK TO SAVEPOINT`?
- What is Oracle's default transaction isolation level? (`READ COMMITTED`)
- What are the four SQL isolation levels? Which does Oracle fully support?
- What is `SERIALIZABLE` isolation in Oracle? How is it implemented? (SCN-based snapshot)
- What is `READ ONLY` transaction?
- What is `SET TRANSACTION`?
- What is Multiversion Concurrency Control (MVCC)?
- How does Oracle implement MVCC? (Undo segments — consistent read using undo data)
- What is a consistent read? What is a statement-level read consistency?
- What is ORA-01555 (Snapshot too old)? What causes it? How do you prevent it?
- What is a read lock in Oracle? Does Oracle use read locks? (No — readers don't block writers, writers don't block readers)
- What is a row lock (TX lock)? When is it acquired?
- What is a DML lock (TM lock)? What are the lock modes?
- What is a DDL lock?
- What is a latch? How does it differ from a lock?
- What is a deadlock in Oracle? What does Oracle do when it detects one? (Rolls back one statement — not the transaction)
- What is lock escalation? Does Oracle escalate row locks? (No — Oracle does not escalate to table locks for DML)
- What is `SELECT ... FOR UPDATE`? What is `SKIP LOCKED`?
- What is `NOWAIT` vs `WAIT n` in `FOR UPDATE`?
- What is an enqueue? What is `V$LOCK`? What is `V$ENQUEUE_STATISTICS`?
- What is distributed transaction? What is 2PC (Two-Phase Commit)?
- What is `DBMS_TRANSACTION`?
- What is autonomous transaction? When should you use it?
- What is flashback query? What is `AS OF TIMESTAMP`? `AS OF SCN`?
- What is the SCN (System Change Number)?

> **Best Practices:**
> - Keep transactions short — long transactions hold undo, increase ORA-01555 risk, and hold locks.
> - Commit frequently in batch jobs — use SAVEPOINT checkpoints to avoid full re-run on failure.
> - Monitor `V$LOCK` and `V$SESSION` to detect blocking sessions.
> - Increase `UNDO_RETENTION` and undo tablespace size if ORA-01555 occurs.
> - Never rely on Oracle's deadlock resolution for application correctness — fix the locking order.
> - Use `SELECT ... FOR UPDATE SKIP LOCKED` for queue-like table processing.

---

## 12. Oracle Partitioning

- What is partitioning? What problem does it solve?
- What are the partitioning types: Range, List, Hash, Composite (Range-Hash, Range-List, List-Hash)?
- What is Range partitioning? Give an example with `PARTITION BY RANGE (date_col)`.
- What is List partitioning?
- What is Hash partitioning? When would you use it?
- What is interval partitioning (Oracle 11g)?? How does it differ from range?
- What is reference partitioning (Oracle 11g)?
- What is virtual column partitioning?
- What is partition pruning? How does the optimizer perform it?
- What is partition-wise join?
- What is local index vs global index on a partitioned table?
- What is a local prefixed index vs local non-prefixed index?
- What is global partitioned index?
- Why do global indexes become unusable after partition maintenance? (`UPDATE INDEXES` clause)
- What is partition exchange? What is `EXCHANGE PARTITION`?
- What is partition split, merge, drop, truncate, add?
- What is `SUBPARTITION`?
- What is `MAXVALUE` partition?
- What is `DEFAULT` partition in list partitioning?
- What is online partition move (Oracle 12c R2)?
- What is `ROW MOVEMENT`?
- What is Interval-Reference partitioning?

> **Best Practices:**
> - Partition large tables by a natural range (date) to enable partition pruning in time-based queries.
> - Always use `UPDATE INDEXES` or `UPDATE GLOBAL INDEXES` after partition maintenance to keep global indexes valid.
> - Use local indexes on partitioned tables — they partition with the table, require no global rebuild.
> - Use `EXCHANGE PARTITION` for fast bulk data loads — load into a staging table, exchange in.
> - Enable `ROW MOVEMENT` if rows can change their partition key value.

---

## 13. Oracle Security

- What is a schema? What is the difference between a user and a schema in Oracle?
- What is `CREATE USER`? What are user attributes: password, default tablespace, temporary tablespace, profile?
- What is a profile? What can it enforce? (Password policy, resource limits)
- What is `FAILED_LOGIN_ATTEMPTS`? `PASSWORD_LOCK_TIME`? `PASSWORD_LIFE_TIME`?
- What is a privilege? What are system privileges vs object privileges?
- What is `GRANT` and `REVOKE`?
- What is `WITH GRANT OPTION`? What is `WITH ADMIN OPTION`?
- What is a role? What are predefined roles: `DBA`, `CONNECT`, `RESOURCE`, `SELECT_CATALOG_ROLE`?
- What is `PUBLIC`? What is a public synonym?
- What is row-level security? What is Virtual Private Database (VPD)?
- What is `DBMS_RLS`? What is a security policy function?
- What is Oracle Label Security (OLS)?
- What is Oracle Database Vault?
- What is Oracle Audit Vault and Database Firewall?
- What is unified auditing (Oracle 12c)?
- What is `AUDIT` statement? What is fine-grained auditing (`DBMS_FGA`)?
- What is transparent data encryption (TDE)?
- What is data masking?
- What is Oracle Advanced Security Option?
- What is `SYS` vs `SYSTEM` user? When should you use them?
- What is `SYSDBA` vs `SYSOPER` privilege?
- What is the password file (`orapw`)?
- What is `CONNECT / AS SYSDBA`?
- What is SQL injection prevention in Oracle? (Bind variables, `DBMS_ASSERT`)
- What is `DBMS_ASSERT.ENQUOTE_LITERAL`? `ENQUOTE_NAME`?

> **Best Practices:**
> - Apply principle of least privilege — grant only what is needed.
> - Never use `SYS` or `DBA` role for application connections.
> - Use VPD (RLS) for multi-tenant applications to enforce row-level data isolation.
> - Enable unified auditing for compliance (SOX, PCI-DSS, HIPAA).
> - Use TDE for data-at-rest encryption of sensitive tablespaces.
> - Use bind variables in dynamic SQL (`DBMS_SQL` or `EXECUTE IMMEDIATE` with `USING`) — prevents SQL injection.

---

## 14. Oracle Backup & Recovery

- What is Oracle Recovery Manager (RMAN)?
- What are the RMAN backup types: full, incremental (level 0, level 1), cumulative incremental?
- What is an image copy vs backup set?
- What is a backup piece?
- What is the RMAN catalog vs control file as repository?
- What is the Fast Recovery Area (FRA)?
- What is `ARCHIVELOG` mode vs `NOARCHIVELOG` mode?
- What is the difference between `BACKUP DATABASE` and `BACKUP AS COPY DATABASE`?
- What is block change tracking? How does it speed up incremental backups?
- What is `RECOVER DATABASE`? What is `RECOVER DATAFILE`?
- What is `RESTORE DATABASE`? What is `RESTORE DATAFILE`?
- What is incomplete recovery (point-in-time recovery)?
- What is `RECOVER DATABASE UNTIL TIME/SCN/CANCEL`?
- What is `RESETLOGS`? When is it required?
- What is Oracle Data Guard?
- What is a physical standby vs logical standby vs snapshot standby?
- What is `REDO APPLY` (physical standby) vs `SQL APPLY` (logical standby)?
- What is Maximum Protection, Maximum Availability, Maximum Performance (Data Guard modes)?
- What is `SWITCHOVER` vs `FAILOVER` in Data Guard?
- What is Active Data Guard (read-only standby)?
- What is Oracle GoldenGate? How does it differ from Data Guard?
- What is Flashback Database? How does it differ from point-in-time recovery?
- What is Flashback Table? `FLASHBACK TABLE ... TO BEFORE DROP`?
- What is the Recycle Bin in Oracle? `FLASHBACK TABLE ... TO BEFORE DROP`?
- What is `PURGE RECYCLEBIN`?
- What is Flashback Data Archive (FDA / Total Recall)?
- What is `DBMS_FLASHBACK`?
- What is Oracle Zero Data Loss Recovery Appliance (ZDLRA)?

> **Best Practices:**
> - Always run in `ARCHIVELOG` mode for production databases — enables point-in-time recovery.
> - Enable block change tracking — significantly reduces incremental backup time.
> - Test recovery regularly — an untested backup is not a backup.
> - Configure Data Guard for HA and DR — RPO near-zero with synchronous redo shipping.
> - Set FRA large enough to hold all recovery files — configure `DB_RECOVERY_FILE_DEST_SIZE`.

---

## 15. Oracle Performance Tuning

- What is wait event analysis? What are Oracle wait events?
- What is `V$SESSION_WAIT`? `V$SYSTEM_EVENT`? `V$SESSION_EVENT`?
- What are the top wait events to investigate?
  - `db file sequential read` (index scan — single block)
  - `db file scattered read` (full table scan — multi-block)
  - `log file sync` (commit wait — LGWR)
  - `buffer busy waits` (hot blocks)
  - `latch: cache buffers chains` (buffer cache latch contention)
  - `enq: TX - row lock contention` (blocking locks)
  - `direct path read/write` (direct I/O, parallel query, temp)
- What is the Oracle wait interface?
- What is AWR (Automatic Workload Repository)? How often does it snapshot?
- What is a Statspack report? Is it still used?
- What is ASH (Active Session History)? What is `V$ACTIVE_SESSION_HISTORY`?
- What is ADDM (Automatic Database Diagnostic Monitor)?
- What is a SQL Tuning Advisor (`DBMS_SQLTUNE`)?
- What is the SQL Access Advisor (`DBMS_ADVISOR`)?
- What is SQL Plan Management (SPM)? What is a SQL plan baseline?
- What is the Oracle 12c Adaptive Query Optimization?
- What is the `/*+ GATHER_PLAN_STATISTICS */` hint?
- What is `V$SQL` — `ELAPSED_TIME`, `CPU_TIME`, `BUFFER_GETS`, `DISK_READS`, `EXECUTIONS`?
- What is `BUFFER_GETS / EXECUTIONS` ratio? What is logical I/O vs physical I/O?
- What is a high-water mark scan? How do you fix it?
- What is chained rows? What is row migration?
- What is block contention? What causes `buffer busy waits`?
- What is free list contention? How does ASSM solve it?
- What is redo log contention (`log file sync`)? How do you tune it?
- What is `PCTFREE`? `PCTUSED`? (Only for manual segment space management)
- What is `INITRANS`? What does it control?
- What is segment shrink? `ALTER TABLE ... SHRINK SPACE`?
- What is online table redefinition (`DBMS_REDEFINITION`)?
- What is parallel query? What is DOP (Degree of Parallelism)?
- What is `PARALLEL` hint? `PARALLEL_DEGREE_POLICY`?
- What is result cache? `RESULT_CACHE` hint? `DBMS_RESULT_CACHE`?
- What is the SQL result cache vs PL/SQL function result cache?
- What is In-Memory Column Store (Oracle 12c)?

> **Best Practices:**
> - Always tune top wait events first — don't tune SQL that isn't a bottleneck.
> - Focus on high `ELAPSED_TIME * EXECUTIONS` SQL — total impact, not just per-execution cost.
> - Gather fresh statistics before performance investigation — stale stats cause misleading plans.
> - Use `DBMS_SQLTUNE.CREATE_TUNING_TASK` for automated SQL tuning recommendations.
> - Fix locking issues before SQL tuning — a 5-second lock wait overwhelms any SQL optimization.
> - Use `PCTFREE` wisely — too low causes row chaining for UPDATE-heavy tables.

---

## 16. Oracle High Availability & RAC

- What is Oracle RAC (Real Application Clusters)?
- What is Cache Fusion? How does it work?
- What is an interconnect in RAC?
- What is a global cache (GCS) vs global enqueue service (GES)?
- What is `gc buffer busy acquire/release` wait event?
- What is `db file sequential read` vs `gc cr block received` in RAC?
- What is node affinity / service affinity in RAC?
- What is `SRVCTL`? What is it used for?
- What is Oracle Clusterware? What is OCR (Oracle Cluster Registry)? What is VD (Voting Disk)?
- What is `css` (Cluster Synchronization Services)?
- What is a split-brain scenario in RAC? How is it prevented? (Voting disk — node eviction)
- What is node eviction? What triggers it?
- What is an Oracle Service? How do you configure load balancing and failover?
- What is `TAF` (Transparent Application Failover)?
- What is Application Continuity (Oracle 12c)?
- What is Oracle Flex ASM?
- What is Oracle Flex Clusters (Hub/Leaf)?
- What is Oracle Data Guard FAR SYNC instance?
- What is Oracle Maximum Availability Architecture (MAA)?
- What is `SCAN` (Single Client Access Name)?

> **Best Practices:**
> - Design applications for RAC — avoid sequences without `CACHE`, global temp tables, or heavy cross-instance block transfers.
> - Use services to direct workloads to specific RAC nodes — enables affinity and reduces Cache Fusion traffic.
> - Monitor `gc buffer busy` waits — indicates hot blocks being transferred across interconnect.
> - Test node eviction scenarios — ensure application reconnects correctly.
> - Use Application Continuity for mission-critical workloads — transparent replay after outages.

---

## 17. Oracle Advanced Features

- What is Oracle Streams (deprecated)? What replaced it? (GoldenGate, XStream)
- What is Oracle Advanced Queuing (AQ)? What is `DBMS_AQ`?
- What is a queue table? What is a queue? What is a subscriber?
- What is Oracle Scheduler (`DBMS_SCHEDULER`)? What replaced `DBMS_JOB`?
- What is a job chain in Oracle Scheduler?
- What is `DBMS_PARALLEL_EXECUTE`? When is it used?
- What is Oracle Text (Oracle interMedia Text)?
- What is `CONTEXT` index? What is `CTXSYS`?
- What is Oracle Spatial (SDO)?
- What is Oracle OLAP? What is an analytic workspace?
- What is Oracle Data Mining (ODM)?
- What is Oracle XML DB? What is `XMLTYPE`? What is `XMLQUERY`, `XMLTABLE`?
- What is `JSON_TABLE`, `JSON_VALUE`, `JSON_QUERY`, `JSON_OBJECT` (Oracle 12c+)?
- What is `IS JSON` constraint?
- What is Oracle Database In-Memory? What is an In-Memory Column Unit (IMCU)?
- What is dual-format (row store + column store)?
- What is In-Memory Compression? What is In-Memory Expression?
- What is Oracle Sharding (Oracle 12c R2)?
- What is `DBMS_LOCK`? What is a user-defined lock?
- What is `DBMS_PIPE`?
- What is `UTL_HTTP`, `UTL_FILE`, `UTL_SMTP`, `UTL_TCP`?
- What is `DBMS_CRYPTO`?
- What is `DBMS_OBFUSCATION_TOOLKIT` (deprecated)?
- What is Oracle Workspace Manager (`DBMS_WM`)?
- What is `DBMS_METADATA`? When is it useful?
- What is `DBMS_DDL`?
- What is Oracle Change Data Capture (CDC)?
- What is LogMiner (`DBMS_LOGMNR`)? What does it do?
- What is XStream API?

> **Best Practices:**
> - Use Oracle Scheduler (`DBMS_SCHEDULER`) over `DBMS_JOB` — better monitoring, chains, events.
> - Use AQ for reliable async messaging within Oracle — native, transactional, exactly-once.
> - Use In-Memory Column Store for analytics on OLTP data — eliminates separate DW for ad-hoc queries.
> - Use LogMiner for change data capture and auditing — reads redo logs non-invasively.

---

## 18. Oracle Data Dictionary & Dynamic Performance Views

- What is the Oracle Data Dictionary? Where is it stored? (SYSTEM tablespace — SYS-owned tables)
- What are the prefixes: `DBA_`, `ALL_`, `USER_`? What is the difference?
- What is `V$` (V-dollar) views? What are they backed by? (In-memory structures, X$ tables)
- What is `GV$` views in RAC? (Global V$ — across all instances)
- What is `X$` tables? Can application users query them?
- Key dictionary views:
  - `DBA_TABLES`, `DBA_COLUMNS`, `DBA_INDEXES`, `DBA_CONSTRAINTS`, `DBA_TRIGGERS`
  - `DBA_OBJECTS`, `DBA_SOURCE`, `DBA_DEPENDENCIES`
  - `DBA_USERS`, `DBA_ROLES`, `DBA_SYS_PRIVS`, `DBA_TAB_PRIVS`, `DBA_ROLE_PRIVS`
  - `DBA_SEGMENTS`, `DBA_EXTENTS`, `DBA_FREE_SPACE`
  - `DBA_TABLESPACES`, `DBA_DATA_FILES`, `DBA_TEMP_FILES`
  - `DBA_TAB_STATISTICS`, `DBA_TAB_COL_STATISTICS`, `DBA_TAB_HISTOGRAMS`
- Key performance views:
  - `V$SESSION`, `V$SQL`, `V$SQLAREA`, `V$SQL_PLAN`, `V$SQL_PLAN_STATISTICS`
  - `V$SESSION_WAIT`, `V$SYSTEM_EVENT`, `V$SESSION_EVENT`
  - `V$LOCK`, `V$ENQUEUE_STATISTICS`
  - `V$BUFFER_POOL_STATISTICS`, `V$DB_CACHE_ADVICE`
  - `V$SGASTAT`, `V$PGASTAT`, `V$PROCESS`
  - `V$LOG`, `V$LOGFILE`, `V$ARCHIVED_LOG`
  - `V$TABLESPACE`, `V$DATAFILE`, `V$CONTROLFILE`
  - `V$INSTANCE`, `V$DATABASE`, `V$VERSION`
  - `V$PARAMETER`, `V$SPPARAMETER`
  - `V$ASH` / `V$ACTIVE_SESSION_HISTORY`
  - `DBA_HIST_*` (AWR historical views)

> **Best Practices:**
> - Use `DBA_` views for administration and `ALL_`/`USER_` views for application queries.
> - Query `V$SQL` sorted by `ELAPSED_TIME / EXECUTIONS` — identifies the worst per-call SQL.
> - Monitor `V$LOCK` with blocking session joins to quickly identify deadlocks and lock contention.
> - Use `DBA_TAB_STATISTICS.LAST_ANALYZED` to check statistics freshness.

---

## 19. Oracle 12c / 18c / 19c / 21c / 23c — Version Features

### Oracle 12c (2013) — Multitenant & Pluggable Databases
- CDB/PDB architecture
- Adaptive Query Optimization (adaptive plans, adaptive statistics)
- SQL Plan Directives
- Invisible columns
- `IDENTITY` column (auto-increment — replaces sequences for simple cases)
- `DEFAULT ON NULL`
- `TOP-N` queries: `FETCH FIRST n ROWS ONLY` / `OFFSET n ROWS FETCH NEXT n ROWS ONLY`
- Lateral joins (`LATERAL`, `CROSS APPLY`, `OUTER APPLY`)
- Temporal tables (valid time / flashback archive)
- Online partition move
- Full transportable export/import
- In-Memory Column Store (12.1.0.2)
- JSON support in SQL and PL/SQL

### Oracle 18c (2018)
- Polymorphic Table Functions (PTF)
- Private Temporary Tables (PTT)
- Active Directory integration
- Online index rebuilds without locks
- Approximate query processing (`APPROX_COUNT_DISTINCT`)

### Oracle 19c (2019) — Long Term Release (LTS)
- Automatic indexing (`DBMS_AUTO_INDEX`)
- Real-Time Statistics (online statistics during DML)
- SQL Quarantine (quarantine runaway SQL)
- `LISTAGG ON OVERFLOW` clause
- `JSON_OBJECT`, `JSON_ARRAY`, `JSON_ARRAYAGG`
- Data Guard DML redirection on standby
- Hint Report in execution plan
- SQL Macros (preview)
- Gradual password rollover

### Oracle 21c (2021)
- Native JSON data type
- Blockchain tables (tamper-resistant)
- Immutable tables
- SQL Macros (stable release)
- AutoML (in-database machine learning)
- JavaScript stored procedures (MLE — Multilingual Engine)
- In-Memory enhancements

### Oracle 23c / 23ai (2023–2024)
- `BOOLEAN` data type in SQL (finally!)
- `IF [NOT] EXISTS` in DDL (`CREATE TABLE IF NOT EXISTS`, `DROP TABLE IF EXISTS`)
- Direct joins in UPDATE and DELETE
- `FROM` clause in `SELECT` without table (`SELECT 1+1` without `DUAL`)
- Table value constructor (`VALUES` as a row source)
- Schema-level privileges
- Property Graph support (SQL/PGQ)
- AI Vector Search (vector data type, `VECTOR_DISTANCE`)
- `DOMAIN` type (column domains)
- Annotations on schema objects
- Interval date shorthand

> **Best Practices:**
> - Upgrade to Oracle 19c for LTS stability — it is the most battle-tested recent release.
> - Use `FETCH FIRST n ROWS ONLY` (12c+) instead of `ROWNUM` — ANSI standard and more readable.
> - Use `IDENTITY` columns (12c+) instead of sequence + trigger pattern.
> - Enable Automatic Indexing (19c) in assessment mode first — review recommendations before full enable.
> - Use Real-Time Statistics (19c) for highly volatile tables — stale stats cause plan regressions.

---

## 20. Tricky Senior-Level Oracle Questions

- What is the difference between `TRUNCATE` and `DELETE`? Which can be rolled back?
- What is `DROP TABLE` vs `TRUNCATE TABLE`? Where does `DROP TABLE` go? (Recycle Bin if enabled)
- What is the difference between `WHERE` and `HAVING`? Can you use analytic functions in `WHERE`?
- What is `NULL` in SQL? What is three-valued logic?
- What is `NULL = NULL`? What is the result? (`UNKNOWN` — use `IS NULL`)
- What is `NOT IN (SELECT ... WHERE col IS NULL)`? (Returns no rows — NULL propagation)
- What is the difference between `CHAR` comparison and `VARCHAR2` comparison? (Blank-padded vs non-padded)
- What is `DECODE(val, NULL, 'yes', 'no')`? Does it correctly detect NULL? (Yes — DECODE uses equality with NULL)
- What is `NVL` vs `NVL2` vs `COALESCE`? What is the evaluation difference between `NVL` and `COALESCE`?
- What is a row that exists in the table but is not returned by any query? (Row in an unusable partition, row beyond HWM — not applicable normally; trick: IOT overflow)
- What is the `ROWNUM` trap? Why does `WHERE ROWNUM > 1` return no rows?
- What is `ROWNUM` vs `ROW_NUMBER()`? Can you use `ROWNUM` in `ORDER BY` and get top-N correctly? (No — wrap in subquery)
- What is the difference between `COMMIT` and `DDL` in Oracle? (DDL auto-commits before and after)
- What is the commit behavior when a session disconnects abnormally? (PMON rolls back uncommitted work)
- What is the difference between `REVOKE role FROM user` and `REVOKE system_priv FROM user`?
- What is the `WITH ADMIN OPTION` trap? (Revoking a role does NOT cascade to users who were granted the role `WITH ADMIN OPTION`)
- What is deferred constraint checking? What is `DEFERRABLE INITIALLY DEFERRED`?
- What is `SET CONSTRAINT ALL DEFERRED`?
- What is a circular foreign key? Can Oracle have one?
- What is an autonomous transaction inside a trigger? What problems can it cause?
- What is a serialization failure (ORA-08177) in SERIALIZABLE isolation?
- What is the difference between `SHRINK SPACE` and `MOVE` for reclaiming space?
- What is a hot backup? Can you perform it without RMAN? (ALTER TABLESPACE ... BEGIN BACKUP)

---

## 21. "Explain the Output" — Oracle SQL Puzzles

```sql
-- Q1 — NULL in NOT IN
SELECT * FROM dual WHERE 1 NOT IN (2, NULL);
-- Returns: no rows
-- NULL makes NOT IN return UNKNOWN — any comparison with NULL is UNKNOWN

-- Q2 — ROWNUM trap
SELECT * FROM (SELECT * FROM employees ORDER BY salary DESC)
WHERE ROWNUM > 5 AND ROWNUM <= 10;
-- Returns: no rows — ROWNUM > 5 is never true (ROWNUM starts at 1)
-- Fix: SELECT * FROM (SELECT ..., ROWNUM rn FROM ...) WHERE rn > 5 AND rn <= 10

-- Q3 — DDL auto-commit
BEGIN
  INSERT INTO t VALUES (1);
  EXECUTE IMMEDIATE 'CREATE TABLE x (id NUMBER)';
  ROLLBACK;
END;
-- Row 1 is COMMITTED — DDL auto-commits before execution
-- ROLLBACK has nothing to roll back

-- Q4 — CHAR blank-padding comparison
SELECT * FROM dual WHERE CAST('Y' AS CHAR(3)) = 'Y';
-- Returns: 1 row — Oracle blank-pads CHAR comparisons (Y = 'Y  ')
-- If using VARCHAR2: 'Y' != 'Y  '

-- Q5 — ROWNUM ordering
SELECT rownum, name FROM employees WHERE rownum <= 3 ORDER BY name;
-- ROWNUM is assigned BEFORE ORDER BY — result is 3 arbitrary rows, then sorted
-- NOT the top 3 alphabetically

-- Q6 — DECODE with NULL
SELECT DECODE(NULL, NULL, 'null-match', 'no-match') FROM dual;
-- Returns: 'null-match' — DECODE is the only Oracle function that treats NULL = NULL as true

-- Q7 — Implicit type conversion
CREATE TABLE t (n NUMBER);
INSERT INTO t VALUES (1);
SELECT * FROM t WHERE n = '1';   -- Works — Oracle converts '1' to 1
SELECT * FROM t WHERE n = 'abc'; -- ORA-01722: invalid number
-- Implicit conversion can prevent index use and cause runtime errors

-- Q8 — Sequence NEXTVAL in insert
INSERT INTO t VALUES (seq.NEXTVAL);
INSERT INTO t VALUES (seq.NEXTVAL);
ROLLBACK;
-- Rows rolled back, but sequence values (e.g., 1, 2) are consumed — gaps in sequences are normal

-- Q9 — DELETE vs TRUNCATE rollback
DELETE FROM t;
ROLLBACK;
SELECT COUNT(*) FROM t; -- Rows restored — DELETE is DML, rollback-able

TRUNCATE TABLE t;
ROLLBACK;
SELECT COUNT(*) FROM t; -- 0 rows — TRUNCATE is DDL, cannot be rolled back

-- Q10 — Analytic function in WHERE
SELECT * FROM employees WHERE ROW_NUMBER() OVER (ORDER BY salary) = 1;
-- ORA-30483: window functions not allowed here
-- Fix: wrap in subquery or CTE

-- Q11 — NOT EXISTS vs NOT IN with NULLs
-- employees table: mgr_id column can be NULL
SELECT * FROM departments d
WHERE NOT EXISTS (SELECT 1 FROM employees e WHERE e.dept_id = d.dept_id);
-- Correct — NOT EXISTS handles NULLs properly

SELECT * FROM departments d
WHERE d.dept_id NOT IN (SELECT dept_id FROM employees WHERE dept_id IS NOT NULL);
-- Correct only if you explicitly filter NULLs from subquery

SELECT * FROM departments d
WHERE d.dept_id NOT IN (SELECT dept_id FROM employees);
-- Returns no rows if ANY employee has NULL dept_id — NULL propagation trap

-- Q12 — Trigger mutating table
CREATE OR REPLACE TRIGGER t_aft AFTER INSERT ON orders FOR EACH ROW
BEGIN
  SELECT COUNT(*) INTO :new.order_count FROM orders; -- ORA-04091: mutating table
END;
-- Cannot query the table being modified in a row-level trigger
-- Fix: use compound trigger or autonomous transaction (with care)

-- Q13 — Partition pruning
-- Table partitioned by RANGE on order_date
SELECT * FROM orders WHERE TRUNC(order_date) = DATE '2024-01-01';
-- Partition pruning DISABLED — function on partition key prevents pruning
-- Fix: WHERE order_date >= DATE '2024-01-01' AND order_date < DATE '2024-01-02'

-- Q14 — CONNECT BY cycle
SELECT LEVEL, emp_id, mgr_id
FROM employees
START WITH mgr_id IS NULL
CONNECT BY PRIOR emp_id = mgr_id;
-- ORA-01436 if circular reference exists (emp is their own manager)
-- Fix: CONNECT BY NOCYCLE PRIOR emp_id = mgr_id

-- Q15 — Index not used due to implicit conversion
-- Column: customer_id VARCHAR2(10), indexed
SELECT * FROM customers WHERE customer_id = 12345; -- passing NUMBER
-- Oracle converts: TO_NUMBER(customer_id) = 12345 — index NOT used (function on column)
-- Fix: WHERE customer_id = '12345' (pass string literal)
```

> **Key Rules to Memorise:**
> - `NOT IN` with a subquery that returns NULL gives no rows — use `NOT EXISTS`.
> - `ROWNUM > n` always returns no rows — ROWNUM is assigned before filtering.
> - DDL auto-commits — any pending DML before DDL is committed automatically.
> - `DECODE` is the only function where `NULL = NULL` evaluates to true.
> - Functions on indexed columns prevent index use — use range predicates or FBI.
> - `TRUNCATE` cannot be rolled back — DDL commits immediately.
> - Implicit type conversion can silently prevent index usage — always match data types.
> - Sequence gaps after ROLLBACK are normal — sequences never roll back.

---

## 22. Oracle vs Other Databases — Senior Mental Model

| Concept | Oracle | PostgreSQL | SQL Server | MySQL |
|---|---|---|---|---|
| Auto-increment | `IDENTITY` (12c+) / Sequence+Trigger | `SERIAL` / `IDENTITY` | `IDENTITY` | `AUTO_INCREMENT` |
| String concat | `||` or `CONCAT` | `||` or `CONCAT` | `+` or `CONCAT` | `CONCAT` or `||` |
| Limit rows | `FETCH FIRST n ROWS ONLY` (12c+) / `ROWNUM` | `LIMIT n` | `TOP n` | `LIMIT n` |
| NULL handling | Three-valued logic, DECODE NULL-safe | Same | Same | Same |
| Outer join syntax | `(+)` (old) / ANSI JOIN | ANSI JOIN only | ANSI JOIN | ANSI JOIN |
| String type | `VARCHAR2` | `VARCHAR` | `VARCHAR`/`NVARCHAR` | `VARCHAR` |
| Boolean in SQL | Oracle 23c only | `BOOLEAN` native | `BIT` | `TINYINT(1)` |
| Procedural language | PL/SQL | PL/pgSQL | T-SQL | Stored procedures |
| Partitioning | Native, comprehensive | Native (11+) | Enterprise edition | Native (5.7+) |
| Parallelism | Built-in, parallel DML | Limited | Enterprise only | Limited |
| MVCC | Undo-based | MVCC (tuple versions) | Version store (TempDB) | InnoDB MVCC |
| Hint syntax | `/*+ HINT */` | N/A (pg_hint_plan ext) | `OPTION (HINT)` | N/A |
| Sequence | `CREATE SEQUENCE` | `CREATE SEQUENCE` | N/A (IDENTITY) | N/A (AUTO_INCREMENT) |
| Dual table | `DUAL` | Not needed | Not needed | Not needed |
| Stored program | Package, Procedure, Function | Function, Procedure | Stored Procedure | Stored Procedure |
| RAC equivalent | Oracle RAC | Citus / logical replication | AG / FCI | MySQL Cluster |
| Explain plan | `EXPLAIN PLAN` + `DBMS_XPLAN` | `EXPLAIN (ANALYZE, BUFFERS)` | `SET STATISTICS IO` | `EXPLAIN` |

---

## 23. Oracle Administration Essentials (Senior Awareness)

- What is `STARTUP` / `SHUTDOWN`? What are the modes: `NOMOUNT`, `MOUNT`, `OPEN`?
- What is `SHUTDOWN NORMAL`, `IMMEDIATE`, `TRANSACTIONAL`, `ABORT`?
- What is `STARTUP RESTRICT`?
- What is `ALTER DATABASE OPEN`? `ALTER DATABASE MOUNT`?
- What is `ALTER SYSTEM` vs `ALTER SESSION`?
- What is `SCOPE=SPFILE`, `SCOPE=MEMORY`, `SCOPE=BOTH` in `ALTER SYSTEM`?
- What is SPFILE vs PFILE (init.ora)?
- What is `DBCA` (Database Configuration Assistant)?
- What is `DBUA` (Database Upgrade Assistant)?
- What is Oracle Data Pump (`expdp`/`impdp`)? How does it differ from `exp`/`imp`?
- What is a transportable tablespace?
- What is `SQL*Loader`? What is external table?
- What is `UTL_EXTERNAL` / external table as an ETL technique?
- What is `GATHER_SCHEMA_STATS`, `GATHER_TABLE_STATS`, `GATHER_DATABASE_STATS` in `DBMS_STATS`?
- What is `DBMS_STATS.SET_TABLE_STATS`? When is it used?
- What is `DBMS_STATS.LOCK_TABLE_STATS`? When would you lock statistics?
- What is `DBMS_STATS.RESTORE_TABLE_STATS`?
- What is Automatic Statistics Gathering (maintenance window job)?
- What is Oracle Enterprise Manager (OEM) / Cloud Control?
- What is Oracle SQL Developer? What is SQL*Plus?
- What is `TKPROF`? What is Oracle trace (`ALTER SESSION SET SQL_TRACE`)?
- What is `10046` event? What are the trace levels?
- What is `10053` event? What does it show? (CBO optimizer trace)
- What is `ORADEBUG`?
- What is alert log? Where is it located? What does it record?
- What is the Automatic Diagnostic Repository (ADR)?
- What is `adrci` utility?

> **Best Practices:**
> - Always use `SHUTDOWN IMMEDIATE` — `ABORT` skips checkpoint and requires instance recovery on restart.
> - Use Data Pump (`expdp`/`impdp`) over legacy `exp`/`imp` — faster, parallel, more options.
> - Lock statistics on frequently purged/loaded tables — prevents optimizer from seeing transient empty/full state.
> - Gather statistics after major data loads before running business queries.
> - Keep alert log monitored — ORA-600, ORA-7445 errors require immediate attention.
> - Use `TKPROF` + `10046` trace for deep SQL analysis — shows parse/execute/fetch times and wait events.
