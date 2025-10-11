
# PL/SQL Study Notes

## 1. What is PL/SQL?
PL/SQL (Procedural Language/SQL) is Oracle's procedural extension to SQL. It allows combining SQL statements with procedural constructs like variables, loops, and conditions.

### Key Points
- It’s stored and executed inside the Oracle Database (server-side).
- Used for writing **stored procedures**, **functions**, **triggers**, and **packages**.
- Offers **error handling**, **variables**, and **control structures** that standard SQL lacks.

---

## 2. Basic Structure of a PL/SQL Block
A **block** is the main unit of code in PL/SQL. It consists of three parts:

```sql
DECLARE
  -- (optional) variable declarations
BEGIN
  -- main executable statements
EXCEPTION
  -- (optional) error handling
END;
/
```

Example:
```sql
SET SERVEROUTPUT ON;

DECLARE
  grade NUMBER := 85;
BEGIN
  DBMS_OUTPUT.PUT_LINE('The grade is: ' || grade);
END;
/
```

---

## 3. Printing Output
To print from a PL/SQL block, you must enable server output:

```sql
SET SERVEROUTPUT ON;
DBMS_OUTPUT.PUT_LINE('Your message here');
```

Example with multiple variables:
```sql
DECLARE
  name VARCHAR2(20) := 'Abdou';
  age NUMBER := 21;
BEGIN
  DBMS_OUTPUT.PUT_LINE('Name: ' || name || '  Age: ' || age);
END;
/
```

---

## 4. Variables and Data Types

### Basic Data Types
| Type | Description | Example |
|------|--------------|----------|
| `NUMBER` | Numeric value | `age NUMBER := 21;` |
| `VARCHAR2(size)` | String with defined size | `name VARCHAR2(30) := 'Abdou';` |
| `DATE` | Date/time | `hire_date DATE := SYSDATE;` |
| `BOOLEAN` | True/False | `is_valid BOOLEAN := TRUE;` |

### Composite Data Types
| Type | Description | Example |
|------|--------------|----------|
| `%TYPE` | Uses the type of a column | `emp_name employees.name%TYPE;` |
| `%ROWTYPE` | Represents an entire row | `emp employees%ROWTYPE;` |

---

## 5. Conditional Statements

### IF Statement
```sql
IF condition THEN
  -- statements
ELSIF another_condition THEN
  -- statements
ELSE
  -- statements
END IF;
```

### CASE Statement
```sql
CASE
  WHEN condition1 THEN
    -- code
  WHEN condition2 THEN
    -- code
  ELSE
    -- code
END CASE;
```

Use `CASE` when you have **many ELSIFs**.

---

## 6. Loops

### FOR Loop
```sql
FOR i IN 2..10 LOOP
  IF MOD(i, 2) = 0 THEN
    DBMS_OUTPUT.PUT_LINE('i = ' || i || ' is even');
  END IF;
END LOOP;
/
```

### WHILE Loop
```sql
DECLARE
  i PLS_INTEGER := 1;
  sum PLS_INTEGER := 0;
BEGIN
  WHILE i <= 100 LOOP
    sum := sum + i;
    i := i + 1;
  END LOOP;
  DBMS_OUTPUT.PUT_LINE('The sum is : ' || sum);
END;
/
```

### FOR Loop with SELECT
```sql
BEGIN
  FOR r IN (SELECT name FROM employees) LOOP
    DBMS_OUTPUT.PUT_LINE(r.name);
  END LOOP;
END;
/
```
✅ *NOTE: The record variable 'r' automatically takes the type %ROWTYPE of the 'employees' table ,this means 'r' has the same structure (columns and data types) as a row in 'employees'.*
 

---

## 7. Summary
- **PL/SQL Block** = DECLARE + BEGIN + EXCEPTION + END
- **DBMS_OUTPUT.PUT_LINE** is used for printing
- **%TYPE** and **%ROWTYPE** reuse database column/row types
- **CASE** is better than many ELSIFs
- **FOR**, **WHILE**, and **CURSOR FOR** loops for repetition

---


# SELECT INTO, BULK COLLECT, and Loops

## 1. SELECT INTO

### Purpose
`SELECT INTO` retrieves column values from a table and stores them directly into PL/SQL variables.  
It is used when the query returns **exactly one row**.

### Syntax
```sql
SELECT column1, column2, ...
INTO variable1, variable2, ...
FROM table_name
WHERE condition;
```

### Example (Single Row)
```sql
SET SERVEROUTPUT ON;

DECLARE
  v_name employees.name%TYPE;
  v_salary employees.salary%TYPE;
BEGIN
  SELECT name, salary
  INTO v_name, v_salary
  FROM employees
  WHERE employee_id = 100;

  DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ', Salary: ' || v_salary);
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('No employee found');
  WHEN TOO_MANY_ROWS THEN
    DBMS_OUTPUT.PUT_LINE('More than one employee matched');
END;
/
```

### Common Exceptions
| Exception | Meaning |
|------------|----------|
| `NO_DATA_FOUND` | No row was returned |
| `TOO_MANY_ROWS` | Query returned more than one row |

### Selecting an Entire Row
```sql
DECLARE
  v_emp employees%ROWTYPE;
BEGIN
  SELECT *
  INTO v_emp
  FROM employees
  WHERE employee_id = 101;

  DBMS_OUTPUT.PUT_LINE('Employee: ' || v_emp.name || ', Dept: ' || v_emp.department_id);
END;
/
```

---

## 2. BULK COLLECT

### What it does
`BULK COLLECT` allows retrieving **multiple rows at once** into a PL/SQL collection.  
It improves performance by reducing context switches between SQL and PL/SQL engines.

### Syntax
```sql
SELECT column_name
BULK COLLECT INTO collection_name
FROM table_name;
```

### Example
```sql
DECLARE
  TYPE name_tab IS TABLE OF employees.name%TYPE;
  l_names name_tab;
BEGIN
  SELECT name
  BULK COLLECT INTO l_names
  FROM employees;

  FOR i IN 1..l_names.COUNT LOOP
    DBMS_OUTPUT.PUT_LINE(l_names(i));
  END LOOP;
END;
/
```

### Pros & Cons
| Pros | Cons |
|------|------|
| Faster for large sets (fewer SQL↔PL/SQL context switches) | Consumes more memory |

---

## 3. BULK COLLECT with LIMIT (for large data)
Use this when you want to fetch in batches to save memory.

```sql
DECLARE
  CURSOR c_names IS SELECT name FROM employees;
  TYPE name_tab IS TABLE OF employees.name%TYPE;
  l_names name_tab;
  l_limit CONSTANT PLS_INTEGER := 1000;
BEGIN
  OPEN c_names;
  LOOP
    FETCH c_names BULK COLLECT INTO l_names LIMIT l_limit;
    EXIT WHEN l_names.COUNT = 0;

    FOR i IN 1..l_names.COUNT LOOP
      DBMS_OUTPUT.PUT_LINE(l_names(i));
    END LOOP;
  END LOOP;
  CLOSE c_names;
END;
/
```

### Why use LIMIT?
It controls memory usage by fetching results in smaller chunks.

---

## 4. Cursor FOR Loop vs BULK COLLECT

| Feature | Cursor FOR Loop | BULK COLLECT |
|----------|------------------|--------------|
| Fetch method | Row by row | Batch (many rows at once) |
| Speed | Slower for large data | Much faster |
| Memory usage | Low | Higher |
| Syntax | Simpler | More code |
| Use case | Small/medium result sets | Large result sets |

### Example — Cursor FOR Loop (simpler)
```sql
BEGIN
  FOR r IN (SELECT name FROM employees) LOOP
    DBMS_OUTPUT.PUT_LINE(r.name);
  END LOOP;
END;
/
```

---

## 5. When to Use Each

- ✅ **Use Cursor FOR Loop** — simple, readable, safe for small datasets.  
- ⚡ **Use BULK COLLECT** — high performance with large datasets.  
- ⚙️ **Use BULK COLLECT + LIMIT** — balance between performance and memory.  
- 💾 **Use FORALL** — for bulk DML operations (INSERT, UPDATE, DELETE).

---

### Summary
- `SELECT INTO` — fetch **one row**.
- `BULK COLLECT` — fetch **many rows** into a collection.
- `LIMIT` — fetch in batches to save memory.
- `FOR r IN (SELECT ...)` — simple loop for small data.
- Use `%TYPE` and `%ROWTYPE` to stay consistent with database schema.
---

## 💡 What is a Cursor?

A **cursor** is a variable that allows access to the **result set** of an SQL query — meaning it can handle **0, 1, or many rows**.

Cursors let you:
- Iterate through each row of a query result.
- Process rows one by one inside a loop.
- Store and manipulate data from multiple rows in PL/SQL programs.
### 📘 Summary: Why we use Cursors in PL/SQL

- The `SELECT ... INTO ...` statement is **not flexible** — it only works when the query returns **exactly one row**.  
- If the query returns **multiple rows**, it causes an error.  
- To handle such cases, we need a **cursor**.

### Example — Cursor 
```plsql
SET SERVEROUTPUT ON;

DECLARE
  CURSOR dept_5 IS
    SELECT * FROM employe WHERE IdDepart = 5;

  Somme PLS_INTEGER := 0;
BEGIN
  FOR ligne IN dept_5 LOOP
    Somme := Somme + ligne.salaire;
  END LOOP;

  DBMS_OUTPUT.PUT_LINE('La somme des salaires est égale à : ' || Somme);
END;
/
```
## SQL Triggers 

### 1. What is a Trigger
A **trigger** is a stored PL/SQL block that is automatically executed (fired) by the database when a specific event occurs on a table, view, or database object.  
It is used to enforce business rules, maintain data integrity, perform automatic actions, or log changes.

---

### 2. Triggering Events (Les événements déclencheurs)
Triggers can be activated by different types of events:

- **LMD (Langage de Manipulation de Données)** triggers:  
  Activated by data manipulation statements such as  
  `INSERT`, `UPDATE`, or `DELETE` on a **table or view**.

- **LDD (Langage de Définition de Données)** triggers:  
  Activated by structure modification statements such as  
  `CREATE`, `ALTER`, or `DROP` on **database objects** (table, index, sequence, etc.).

- **Instance-level triggers:**  
  Activated by **database events** such as:  
  - Database startup (`STARTUP`) or shutdown (`SHUTDOWN`)  
  - Specific errors (`NO_DATA_FOUND`, `DUP_VAL_ON_INDEX`, etc.)  
  - User connection or disconnection events

---

### 3. Trigger Restrictions (Déclencheurs: Restrictions)
- A trigger **cannot validate or manage transactions** — the following commands are **forbidden** inside triggers:
  - `COMMIT`
  - `ROLLBACK`
  - `SAVEPOINT`
  - `SET CONSTRAINT`

- **DDL statements** (`CREATE`, `ALTER`, `DROP`, etc.) are **not allowed** inside triggers.

- It is **recommended** to limit the size of a trigger body to **60 lines of PL/SQL code**,  
  and the **maximum size** cannot exceed **32 KB**.

- To **bypass this limitation**, call **subprograms** (procedures or functions) from inside the trigger.

---

### 4. LMD Triggers — Syntax

```sql
CREATE [OR REPLACE] TRIGGER trigger_name
{BEFORE | AFTER | INSTEAD OF} event_list
[OF column_list] ON table_name
[FOR EACH ROW] [WHEN (condition)]
BEGIN
  -- Trigger body (actions to perform)
END;
/
```

#### 🔹 Explanation

* **BEFORE**: The trigger code runs *before* the triggering event.
* **AFTER**: The trigger code runs *after* the triggering event.
* **INSTEAD OF**: Used specifically for **views** to define what happens instead of the triggering action.
* **event_list**: The triggering events (`INSERT`, `UPDATE`, or `DELETE`).
* **UPDATE OF column_list**: Specifies which columns, when updated, will activate the trigger.
* **FOR EACH ROW**: Makes the trigger fire once per affected row (row-level trigger).
* **WHEN (condition)**: Optional clause that limits trigger execution to certain rows.

---

### 5. Event Grouping (Regroupement d’événements)

In Oracle, you can **group multiple triggering events** within a single trigger definition.
This avoids creating multiple triggers for similar actions on the same table.

**Example:**

```plsql
CREATE OR REPLACE TRIGGER emp_audit
AFTER INSERT OR UPDATE OR DELETE ON employees
BEGIN
  INSERT INTO audit_log VALUES ('Change detected on employees', SYSDATE);
END;
/
```

➡️ This single trigger will activate **after any** of the three events (`INSERT`, `UPDATE`, or `DELETE`) occur on the `employees` table.

## Row-level vs Table-level
- **Row-level Trigger (`FOR EACH ROW`)**
  - Executes **once per affected row**.
  - Can use `:OLD` and `:NEW` values.
  - Can include a `WHEN` condition for specific rows.

- **Table-level Trigger (without `FOR EACH ROW`)**
  - Executes **once per SQL statement**, regardless of how many rows are affected.
  - Does **not** access individual row data.
  - Used for actions applying to the entire table.