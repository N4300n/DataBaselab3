# Lab 12. Querying Data
Создание таблиц:
 
```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50) NOT NULL,
    department_id INT,
    salary DECIMAL(10, 2) NOT NULL
);
 
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);
 
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);
 
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    email VARCHAR(100) NOT NULL
);
 
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    employee_id INT REFERENCES employees(id),
    amount DECIMAL(10, 2) NOT NULL
);
```
 
Вставка данных:
 
```sql
INSERT INTO employees (name, department, department_id, salary) VALUES
('Brad Pitt', 'Sales', 3, 50000.00),
('Brandon Stark', 'Engineering', 5, 80000.00),
('Alice Smith', 'Engineering', 5, 95000.00),
('John Doe', 'Sales', 3, 65000.00),
('Carol Danvers', 'HR', 7, 45000.00);
 
INSERT INTO products (product_name, price) VALUES
('Fresh Strawberry', 25.00),
('Blueberry Jam', 45.00),
('Apple', 10.00),
('Super Computer', 1200.00);
 
INSERT INTO customers (customer_name) VALUES
('Brad Pitt'),
('Brandon Stark'),
('Charlie Brown');
 
INSERT INTO users (email) VALUES
('user1@gmail.com'),
('USER2@GMAIL.COM'),
('dev@company.org'),
('invalid-email@com');
 
INSERT INTO sales (employee_id, amount) VALUES
(2, 1500.00),
(3, 2000.00),
(1, 500.00);
```
 
## 2. Примеры запросов (Querying Data)
 
### A. Базовый SELECT и вычисления с алиасами (AS)
 
```sql
SELECT * FROM employees;
```
 
```sql
SELECT name, department FROM employees;
```
 
```sql
SELECT name, salary, salary * 0.10 AS potential_bonus 
FROM employees;
```
 
### B. Фильтрация WHERE (сравнение, логические операторы, BETWEEN)
 
```sql
SELECT name, salary 
FROM employees 
WHERE department = 'Sales';
```
 
```sql
SELECT name 
FROM employees 
WHERE salary > 75000 AND department = 'Engineering';
```
 
```sql
SELECT product_name, price 
FROM products 
WHERE price BETWEEN 20 AND 50;
```
 
### C. Шаблоны LIKE и ILIKE
 
```sql
SELECT customer_name 
FROM customers 
WHERE customer_name LIKE 'Bra%';
```
 
```sql
SELECT product_name 
FROM products 
WHERE product_name LIKE '%berry';
```
 
```sql
SELECT email 
FROM users 
WHERE email ILIKE '%@gmail.com';
```
 
### D. Регулярные выражения (~ и ~*)
 
```sql
SELECT name 
FROM employees 
WHERE name ~ '^[AB]';
```
 
```sql
SELECT email 
FROM users 
WHERE email ~* '^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$';
```
 
### E. Операторы IN и EXISTS
 
```sql
SELECT name, department 
FROM employees 
WHERE department_id IN (3, 5, 7);
```
 
```sql
SELECT name 
FROM employees 
WHERE id IN (SELECT employee_id FROM sales WHERE amount > 1000);
```
 
```sql
SELECT name 
FROM employees e 
WHERE EXISTS (
    SELECT 1 
    FROM sales s 
    WHERE s.employee_id = e.id AND s.amount > 1000
);
```
 
### F. Условный оператор CASE
 
```sql
SELECT name, salary,
  CASE 
    WHEN salary > 100000 THEN 'Senior'
    WHEN salary BETWEEN 60000 AND 100000 THEN 'Mid-Level'
    ELSE 'Junior'
  END AS employee_level
FROM employees;
```
 
### G. Обобщённые табличные выражения (CTE / WITH)
 
```sql
WITH department_averages AS (
  SELECT 
    department, 
    AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department
)
SELECT 
  e.name, 
  e.salary, 
  e.department, 
  ROUND(da.avg_salary, 2) AS avg_department_salary
FROM employees e
JOIN department_averages da ON e.department = da.department
WHERE e.salary >= da.avg_salary;
```
