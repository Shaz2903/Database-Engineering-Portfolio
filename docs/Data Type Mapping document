# Oracle to PostgreSQL Data Type Mapping Guide

## 1. Overview

This document provides a structured mapping between Oracle and PostgreSQL data types. It is intended for use in database migration projects where schema conversion is required while ensuring data integrity, performance, and compatibility.

---

## 2. Purpose

* Standardize data type conversions between Oracle and PostgreSQL
* Reduce migration errors during schema transformation
* Ensure consistency across large-scale enterprise migrations
* Serve as a reference for developers and database engineers

---

## 3. Core Data Type Mappings

| Oracle Data Type | PostgreSQL Equivalent | Notes                                 |
| ---------------- | --------------------- | ------------------------------------- |
| VARCHAR2(n)      | VARCHAR(n)            | Direct mapping                        |
| CHAR(n)          | CHAR(n)               | Direct mapping                        |
| NUMBER           | NUMERIC               | Use precision/scale where possible    |
| NUMBER(p, s)     | NUMERIC(p, s)         | Recommended for financial data        |
| INTEGER          | INTEGER               | Direct mapping                        |
| DATE             | TIMESTAMP             | PostgreSQL DATE has no time component |
| TIMESTAMP        | TIMESTAMP             | Direct mapping                        |
| CLOB             | TEXT                  | Large text storage                    |
| BLOB             | BYTEA                 | Binary data                           |
| RAW              | BYTEA                 | Binary data                           |
| LONG             | TEXT                  | Deprecated in Oracle                  |

---

## 4. Special Conversions

### 4.1 Boolean Handling

Oracle does not have a BOOLEAN type in SQL tables, while PostgreSQL does.

| Oracle Approach   | PostgreSQL Equivalent |
| ----------------- | --------------------- |
| NUMBER(1) (0/1)   | BOOLEAN               |
| CHAR(1) ('Y'/'N') | BOOLEAN               |

Example conversion logic:

```sql
CASE WHEN status = 'Y' THEN TRUE ELSE FALSE END
```

---

### 4.2 Date and Time Differences

| Oracle              | PostgreSQL                 | Notes             |
| ------------------- | -------------------------- | ----------------- |
| SYSDATE             | CURRENT_TIMESTAMP          | Includes time     |
| TRUNC(SYSDATE)      | CURRENT_DATE               | Removes time      |
| ADD_MONTHS(date, n) | date + INTERVAL 'n months' | PostgreSQL syntax |

---

### 4.3 String Functions

| Oracle   | PostgreSQL    |
| -------- | ------------- |
| NVL(a,b) | COALESCE(a,b) |
| DECODE() | CASE WHEN     |
| SUBSTR() | SUBSTRING()   |
| LENGTH() | LENGTH()      |
| INSTR()  | POSITION()    |

---

### 4.4 Numeric Functions

| Oracle  | PostgreSQL |
| ------- | ---------- |
| ROUND() | ROUND()    |
| TRUNC() | TRUNC()    |
| MOD()   | MOD()      |

---

## 5. Oracle-Specific Types and Handling

### 5.1 RAW / LONG RAW

* Convert to BYTEA in PostgreSQL
* Ensure binary encoding consistency

### 5.2 ROWID

* No direct equivalent
* Replace with PRIMARY KEY or surrogate key

### 5.3 SEQUENCE

* PostgreSQL uses:

```sql
GENERATED AS IDENTITY
```

or:

```sql
CREATE SEQUENCE
```

---

## 6. Migration Considerations

### 6.1 Precision Handling

* Always validate numeric precision during conversion
* Financial systems must preserve decimal accuracy

---

### 6.2 NULL Behavior

* Oracle treats empty string as NULL
* PostgreSQL distinguishes empty string and NULL

This difference must be handled carefully during migration.

---

### 6.3 Case Sensitivity

* Oracle: case-insensitive by default
* PostgreSQL: case-sensitive unless quoted

Recommendation:

* Use lowercase schema naming in PostgreSQL

---

## 7. Performance Considerations

* Use NUMERIC only when necessary (prefer INTEGER for performance)
* Avoid excessive TEXT usage for structured data
* Index frequently queried columns after migration
* Validate query execution plans post-migration

---

## 8. Example Conversion

### Oracle Table

```sql
CREATE TABLE customer (
    customer_id NUMBER,
    name VARCHAR2(100),
    balance NUMBER(10,2),
    created_date DATE,
    status CHAR(1)
);
```

### PostgreSQL Equivalent

```sql
CREATE TABLE customer (
    customer_id INTEGER,
    name VARCHAR(100),
    balance NUMERIC(10,2),
    created_date TIMESTAMP,
    status BOOLEAN
);
```

---

## 9. Conclusion

This mapping guide provides a foundational reference for migrating Oracle database schemas to PostgreSQL. It ensures consistency, reduces migration risk, and supports scalable transformation of enterprise systems.

It should be used alongside validation scripts, migration planning documents, and application compatibility testing.
