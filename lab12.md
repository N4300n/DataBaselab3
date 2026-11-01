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
<img width="459" height="624" alt="image" src="https://github.com/user-attachments/assets/f840a099-938c-4056-88ea-6f55c26c8209" />

 
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

<img width="658" height="594" alt="image" src="https://github.com/user-attachments/assets/76c1f287-1ec6-4a3f-9a02-cbdc8be72e88" />


 
## 2. Примеры запросов (Querying Data)
 
### A. Базовый SELECT и вычисления с алиасами (AS)
 
```sql
SELECT * FROM employees;
```

<img width="679" height="243" alt="image" src="https://github.com/user-attachments/assets/7ead7559-b652-4770-aeee-430b75f5e3b0" />

 
```sql
SELECT name, department FROM employees;
```
<img width="367" height="243" alt="image" src="https://github.com/user-attachments/assets/6b91e630-b236-4975-9fa2-f7b69106e5b8" />

 
```sql
SELECT name, salary, salary * 0.10 AS potential_bonus 
FROM employees;
```

<img width="441" height="242" alt="image" src="https://github.com/user-attachments/assets/495839bc-c771-4fe1-a793-1c57f3584d78" />

 
### B. Фильтрация WHERE (сравнение, логические операторы, BETWEEN)
 
```sql
SELECT name, salary 
FROM employees 
WHERE department = 'Sales';
```
<img width="348" height="172" alt="image" src="https://github.com/user-attachments/assets/497edbd6-51b0-4dc5-bcec-a3cf4ac3c3af" />

 
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
<img width="224" height="167" alt="image" src="https://github.com/user-attachments/assets/427ffb60-ad8b-49bb-a9a0-4f68673090c2" />


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
<img width="224" height="177" alt="image" src="https://github.com/user-attachments/assets/fa3146b5-06d8-4af6-80b3-d18f4c07c276" />

 
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
<img width="235" height="185" alt="image" src="https://github.com/user-attachments/assets/f5f060f8-811f-48ee-8d8a-5cb84bcd6956" />

 
```sql
SELECT email 
FROM users 
WHERE email ~* '^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$';
```
<img width="221" height="198" alt="image" src="https://github.com/user-attachments/assets/446976f6-6b9e-4e8c-a32a-47204cb4b65e" />

### E. Операторы IN и EXISTS
 
```sql
SELECT name, department 
FROM employees 
WHERE department_id IN (3, 5, 7);
```
<img width="364" height="238" alt="image" src="https://github.com/user-attachments/assets/74cec843-0015-4f35-942e-b337397385cf" />

 
```sql
SELECT name 
FROM employees 
WHERE id IN (SELECT employee_id FROM sales WHERE amount > 1000);
```
<img width="219" height="147" alt="image" src="https://github.com/user-attachments/assets/95cfffcf-c0c6-4814-ac4f-c10a9b360ed8" />

 
```sql
SELECT name 
FROM employees e 
WHERE EXISTS (
    SELECT 1 
    FROM sales s 
    WHERE s.employee_id = e.id AND s.amount > 1000
);
```

<img width="215" height="174" alt="image" src="https://github.com/user-attachments/assets/fd581d8a-aba1-43be-86c6-7e83e3b63854" />

 
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
<img width="467" height="254" alt="image" src="https://github.com/user-attachments/assets/f2b139e7-da10-495a-b596-049fe273aba4" />

 
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

<img width="635" height="188" alt="image" src="https://github.com/user-attachments/assets/9216c688-4a59-45eb-8f1c-537b7e622461" />

