title: SQL Joins Cheatsheet
date: "2024-10-28T22:40:32.169Z"
description: A concise cheatsheet on SQL joins
---

### 1. **INNER JOIN**
- **Description**: Returns records that have matching values in both tables.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  INNER JOIN table2 ON table1.column = table2.column;
  ```

### 2. **LEFT JOIN (LEFT OUTER JOIN)**
- **Description**: Returns all records from the left table and matched records from the right table. If there is no match, NULL values are returned for columns from the right table.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  LEFT JOIN table2 ON table1.column = table2.column;
  ```

### 3. **RIGHT JOIN (RIGHT OUTER JOIN)**
- **Description**: Returns all records from the right table and matched records from the left table. If there is no match, NULL values are returned for columns from the left table.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  RIGHT JOIN table2 ON table1.column = table2.column;
  ```

### 4. **FULL JOIN (FULL OUTER JOIN)**
- **Description**: Returns all records when there is a match in either left or right table records. If there are no matches, NULL values are returned for non-matching columns.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  FULL JOIN table2 ON table1.column = table2.column;
  ```

### 5. **CROSS JOIN**
- **Description**: Returns the Cartesian product of both tables, combining each row from the first table with every row from the second table.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  CROSS JOIN table2;
  ```

### 6. **SELF JOIN**
- **Description**: Joins a table to itself to compare rows within the same table.
- **Syntax**:
  ```sql
  SELECT a.columns, b.columns
  FROM table a, table b
  WHERE condition;
  ```

### 7. **JOIN vs. OUTER JOIN**
- **JOIN** typically refers to INNER JOIN unless specified otherwise.
- **OUTER JOIN** includes LEFT, RIGHT, and FULL joins.

### 8. **Using Aliases**
- Aliases can simplify queries and improve readability. Use the `AS` keyword.
- **Example**:
  ```sql
  SELECT a.name AS employee_name, b.department AS department_name
  FROM employees AS a
  INNER JOIN departments AS b ON a.dept_id = b.id;
  ```

### Example Scenario
Assume you have two tables: **employees** and **departments**.

- **employees**: `id`, `name`, `dept_id`
- **departments**: `id`, `department_name`

### Example Queries
1. **INNER JOIN**:
   ```sql
   SELECT e.name, d.department_name
   FROM employees e
   INNER JOIN departments d ON e.dept_id = d.id;
   ```

2. **LEFT JOIN**:
   ```sql
   SELECT e.name, d.department_name
   FROM employees e
   LEFT JOIN departments d ON e.dept_id = d.id;
   ```

3. **RIGHT JOIN**:
   ```sql
   SELECT e.name, d.department_name
   FROM employees e
   RIGHT JOIN departments d ON e.dept_id = d.id;
   ```

4. **FULL JOIN**:
   ```sql
   SELECT e.name, d.department_name
   FROM employees e
   FULL JOIN departments d ON e.dept_id = d.id;
   ```

### Conclusion
Understanding SQL joins is crucial for effective database querying. This cheatsheet provides a quick reference to help you choose the right join type based on your data retrieval needs.