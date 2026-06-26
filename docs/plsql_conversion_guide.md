# Oracle PL/SQL to PostgreSQL Conversion Guide

## 1. Overview

This document explains how Oracle PL/SQL constructs are translated into PostgreSQL equivalents during database migration projects. It focuses on structural, functional, and behavioral differences between the two systems.

---

## 2. Key Architectural Differences

| Oracle                         | PostgreSQL                                 |
| ------------------------------ | ------------------------------------------ |
| PL/SQL is tightly integrated   | Procedural language (PL/pgSQL) is separate |
| Packages supported             | No package concept                         |
| Implicit commits in some cases | Explicit transaction control               |
| Strong dependency chaining     | Schema-level modularity                    |

---

## 3. Packages Conversion

### Oracle Example

```sql id="pck1"
CREATE OR REPLACE PACKAGE pkg_customer AS
  PROCEDURE get_customer;
END;
```

### PostgreSQL Equivalent

PostgreSQL does NOT support packages. Instead:

* Functions are grouped by schema
* Naming conventions are used

```sql id="pck2"
CREATE SCHEMA customer_pkg;
```

```sql id="pck3"
CREATE OR REPLACE FUNCTION customer_pkg.get_customer()
RETURNS VOID AS $$
BEGIN
  -- logic here
END;
$$ LANGUAGE plpgsql;
```

---

## 4. Procedure Conversion

### Oracle

```sql id="prc1"
CREATE PROCEDURE update_balance (
  p_id NUMBER,
  p_amount NUMBER
)
AS
BEGIN
  UPDATE account SET balance = balance + p_amount
  WHERE id = p_id;
END;
```

### PostgreSQL

```sql id="prc2"
CREATE OR REPLACE PROCEDURE update_balance(
  p_id INT,
  p_amount NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
  UPDATE account
  SET balance = balance + p_amount
  WHERE id = p_id;
END;
$$;
```

---

## 5. Function Conversion

### Oracle

```sql id="fn1"
CREATE FUNCTION get_total(p_id NUMBER)
RETURN NUMBER
AS
  v_total NUMBER;
BEGIN
  SELECT SUM(amount)
  INTO v_total
  FROM orders
  WHERE customer_id = p_id;

  RETURN v_total;
END;
```

### PostgreSQL

```sql id="fn2"
CREATE OR REPLACE FUNCTION get_total(p_id INT)
RETURNS NUMERIC
LANGUAGE plpgsql
AS $$
DECLARE
  v_total NUMERIC;
BEGIN
  SELECT SUM(amount)
  INTO v_total
  FROM orders
  WHERE customer_id = p_id;

  RETURN v_total;
END;
$$;
```

---

## 6. Trigger Conversion

### Oracle

```sql id="tr1"
CREATE OR REPLACE TRIGGER trg_before_insert
BEFORE INSERT ON customer
FOR EACH ROW
BEGIN
  :NEW.created_date := SYSDATE;
END;
```

### PostgreSQL

```sql id="tr2"
CREATE OR REPLACE FUNCTION trg_before_insert_fn()
RETURNS TRIGGER AS $$
BEGIN
  NEW.created_date := CURRENT_TIMESTAMP;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

```sql id="tr3"
CREATE TRIGGER trg_before_insert
BEFORE INSERT ON customer
FOR EACH ROW
EXECUTE FUNCTION trg_before_insert_fn();
```

---

## 7. Exception Handling

### Oracle

```sql id="ex1"
EXCEPTION
  WHEN NO_DATA_FOUND THEN
```

### PostgreSQL

```sql id="ex2"
EXCEPTION WHEN OTHERS THEN
```

More granular handling requires custom logic.

---

## 8. Common Function Replacements

| Oracle  | PostgreSQL        |
| ------- | ----------------- |
| NVL     | COALESCE          |
| SYSDATE | CURRENT_TIMESTAMP |
| ROWNUM  | LIMIT             |
| DECODE  | CASE WHEN         |
| DUAL    | (not required)    |

---

## 9. Best Practices

* Avoid direct one-to-one conversion without redesign
* Replace packages with schema-based modular design
* Minimize procedural logic inside DB where possible
* Prefer set-based operations over loops
* Use explicit transactions

---

## 10. Conclusion

Oracle PL/SQL and PostgreSQL PL/pgSQL differ significantly in architecture. Migration requires not just syntax conversion, but also redesign of procedural logic to align with PostgreSQL’s modular and open-source architecture.
