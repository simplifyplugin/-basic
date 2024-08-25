Here are some commonly asked MySQL queries in interviews, along with explanations to help you prepare:

### 1. **Basic SELECT Queries**

**Retrieve all records from a table:**
```sql
SELECT * FROM employees;
```

**Retrieve specific columns:**
```sql
SELECT first_name, last_name FROM employees;
```

### 2. **Filtering Records Using WHERE**

**Retrieve employees from a specific department:**
```sql
SELECT * FROM employees WHERE department = 'Sales';
```

**Retrieve employees with a salary greater than a certain amount:**
```sql
SELECT * FROM employees WHERE salary > 50000;
```

### 3. **Using Aggregate Functions**

**Count the number of employees:**
```sql
SELECT COUNT(*) FROM employees;
```

**Find the maximum salary:**
```sql
SELECT MAX(salary) FROM employees;
```

**Calculate the average salary:**
```sql
SELECT AVG(salary) FROM employees;
```

### 4. **GROUP BY and HAVING Clauses**

**Count employees in each department:**
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department;
```

**Filter departments with more than 10 employees:**
```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department 
HAVING COUNT(*) > 10;
```

### 5. **JOIN Queries**

**Retrieve employee names along with their department names:**
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

**Left Join to include employees without a department:**
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

### 6. **Subqueries**

**Find employees with the highest salary:**
```sql
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

**Find departments with an average salary greater than a certain amount:**
```sql
SELECT department_id 
FROM employees 
GROUP BY department_id 
HAVING AVG(salary) > 60000;
```

### 7. **Complex Queries**

**Find employees who earn more than the average salary in their department:**
```sql
SELECT e.first_name, e.last_name, e.salary 
FROM employees e
INNER JOIN (
    SELECT department_id, AVG(salary) as avg_salary 
    FROM employees 
    GROUP BY department_id
) d ON e.department_id = d.department_id 
WHERE e.salary > d.avg_salary;
```

**Find the second highest salary:**
```sql
SELECT MAX(salary) 
FROM employees 
WHERE salary < (SELECT MAX(salary) FROM employees);
```

**Find all employees who have the same salary:**
```sql
SELECT salary, GROUP_CONCAT(first_name ORDER BY first_name ASC) as employees 
FROM employees 
GROUP BY salary 
HAVING COUNT(*) > 1;
```

### 8. **Self-Join**

**Find pairs of employees who have the same manager:**
```sql
SELECT e1.first_name AS employee1, e2.first_name AS employee2, e1.manager_id 
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.manager_id 
WHERE e1.id <> e2.id;
```

### 9. **Window Functions (Advanced)**

**Calculate the cumulative salary of employees:**
```sql
SELECT first_name, last_name, salary, 
SUM(salary) OVER (ORDER BY salary) as cumulative_salary
FROM employees;
```

**Rank employees by salary within their department:**
```sql
SELECT first_name, last_name, department_id, salary,
RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as rank
FROM employees;
```

### 10. **Updating and Deleting Records**

**Increase salary by 10% for all employees in a department:**
```sql
UPDATE employees 
SET salary = salary * 1.1 
WHERE department = 'Sales';
```

**Delete employees who have not been active for more than a year:**
```sql
DELETE FROM employees 
WHERE last_active_date < DATE_SUB(NOW(), INTERVAL 1 YEAR);
```

---

For advanced users, interview questions often involve complex scenarios that require deep knowledge of SQL features and performance optimization. Here are some tricky MySQL queries and scenarios you might encounter in an interview, along with explanations:

### 1. **Finding the Nth Highest Salary**

**Problem: Find the second highest salary in the employees table.**
```sql
SELECT DISTINCT salary 
FROM employees 
ORDER BY salary DESC 
LIMIT 1 OFFSET 1;
```
**Alternate Approach (Using Subquery):**
```sql
SELECT MAX(salary) 
FROM employees 
WHERE salary < (SELECT MAX(salary) FROM employees);
```

### 2. **Group-Wise Maximum**

**Problem: Find the highest salary in each department.**
```sql
SELECT department_id, MAX(salary) 
FROM employees 
GROUP BY department_id;
```

**Problem: Find the employee with the highest salary in each department.**
```sql
SELECT e.*
FROM employees e
INNER JOIN (
    SELECT department_id, MAX(salary) AS max_salary 
    FROM employees 
    GROUP BY department_id
) d ON e.department_id = d.department_id AND e.salary = d.max_salary;
```

### 3. **Top N Records Per Group**

**Problem: Retrieve the top 3 highest salaries in each department.**
```sql
SELECT e1.department_id, e1.first_name, e1.salary
FROM employees e1
WHERE (
    SELECT COUNT(DISTINCT e2.salary) 
    FROM employees e2
    WHERE e2.department_id = e1.department_id AND e2.salary >= e1.salary
) <= 3;
```

### 4. **Identify and Remove Duplicates**

**Problem: Find duplicate records in a table.**
```sql
SELECT column_name, COUNT(*) 
FROM table_name 
GROUP BY column_name 
HAVING COUNT(*) > 1;
```

**Problem: Delete duplicate records while keeping the original.**
```sql
DELETE e1 FROM employees e1
INNER JOIN employees e2 
WHERE e1.id > e2.id 
AND e1.column_name = e2.column_name;
```

### 5. **Handling Gaps in Sequences**

**Problem: Find the missing number(s) in a sequence of IDs.**
```sql
SELECT t1.id + 1 AS missing_id
FROM employees t1 
LEFT JOIN employees t2 ON t1.id + 1 = t2.id
WHERE t2.id IS NULL;
```

### 6. **Dynamic Pivot Tables**

**Problem: Convert rows into columns dynamically (pivot operation).**
```sql
SELECT 
    employee_id,
    MAX(CASE WHEN skill = 'Java' THEN level END) AS Java,
    MAX(CASE WHEN skill = 'Python' THEN level END) AS Python,
    MAX(CASE WHEN skill = 'SQL' THEN level END) AS SQL
FROM employee_skills
GROUP BY employee_id;
```

### 7. **Recursive Queries (Common Table Expressions - CTEs)**

**Problem: Generate a hierarchical structure or organizational chart.**
```sql
WITH RECURSIVE EmployeeCTE AS (
    SELECT id, name, manager_id 
    FROM employees 
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.id, e.name, e.manager_id
    FROM employees e
    INNER JOIN EmployeeCTE c ON e.manager_id = c.id
)
SELECT * FROM EmployeeCTE;
```

### 8. **Ranking Without Gaps**

**Problem: Rank employees by salary without gaps (dense rank).**
```sql
SELECT first_name, last_name, salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM employees;
```

### 9. **Rolling Sum/Average**

**Problem: Calculate the rolling sum of salaries ordered by employee hire date.**
```sql
SELECT first_name, hire_date, salary, 
    SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS rolling_sum
FROM employees;
```

### 10. **Performance Optimization**

**Problem: Optimize a slow query that joins large tables.**
- **Indexing:** Ensure appropriate indexes are applied to columns used in JOIN, WHERE, and ORDER BY clauses.
```sql
CREATE INDEX idx_employee_department ON employees(department_id);
```
- **Use EXPLAIN to analyze query execution:**
```sql
EXPLAIN SELECT e.first_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE e.salary > 50000;
```
- **Avoid SELECT *:** Select only the necessary columns to reduce data retrieval overhead.

### 11. **Conditional Aggregation**

**Problem: Count the number of employees in each department who earn above a certain threshold.**
```sql
SELECT department_id,
    COUNT(*) AS total_employees,
    SUM(CASE WHEN salary > 60000 THEN 1 ELSE 0 END) AS high_earners
FROM employees
GROUP BY department_id;
```

### 12. **Time-Based Queries**

**Problem: Retrieve records for the last 7 days.**
```sql
SELECT * FROM employees 
WHERE hire_date >= CURDATE() - INTERVAL 7 DAY;
```

**Problem: Calculate the difference between two timestamps in hours.**
```sql
SELECT TIMESTAMPDIFF(HOUR, start_time, end_time) AS hours_diff
FROM time_log;
```
