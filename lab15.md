# Advanced Querying

## 1. Вложенные запросы (Subqueries)
 
Скалярный подзапрос: поиск сотрудников с зарплатой выше средней.
 
```sql
SELECT first_name, last_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```
<img width="481" height="183" alt="image" src="https://github.com/user-attachments/assets/a12f5bee-05aa-4e7e-a7e6-0ea59120abfa" />

 
## 2. Обобщённые табличные выражения (CTE)
 
Базовый CTE:
 
```sql
WITH dept_salaries AS (
    SELECT department, SUM(salary) AS total_dept_salary
    FROM employees
    GROUP BY department
)
SELECT department, total_dept_salary
FROM dept_salaries
WHERE total_dept_salary > 100000;
```
<img width="350" height="193" alt="image" src="https://github.com/user-attachments/assets/171e1d26-b862-4f55-b5f3-e2e899060fed" />

 
Рекурсивный CTE: построение оргструктуры.
 
```sql
WITH RECURSIVE org_chart AS (

    SELECT employee_id, first_name, last_name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
 
    UNION ALL
 
    SELECT e.employee_id, e.first_name, e.last_name, e.manager_id, oc.level + 1
    FROM employees e
    INNER JOIN org_chart oc ON e.manager_id = oc.employee_id
)
SELECT * FROM org_chart;
```
<img width="644" height="258" alt="image" src="https://github.com/user-attachments/assets/e84658f2-12f4-43b3-8ebe-34f5a6dfaca3" />

 
## 3. Операции над множествами (Set Operations)
 
`UNION` — все уникальные товары из обоих складов:
 
```sql
SELECT product_name FROM warehouse_1
UNION
SELECT product_name FROM warehouse_2;
```
<img width="269" height="227" alt="image" src="https://github.com/user-attachments/assets/0920b91d-f3a0-4a72-95d1-27f3b9c5119a" />

 
`UNION ALL` — все товары с сохранением дубликатов:
 
```sql
SELECT product_name FROM warehouse_1
UNION ALL
SELECT product_name FROM warehouse_2;
```
<img width="235" height="260" alt="image" src="https://github.com/user-attachments/assets/09542d4f-429f-4aa9-8312-1b318a107abb" />

 
`INTERSECT` — товары с нулевым остатком на обоих складах:
 
```sql
SELECT product_id FROM warehouse_1 WHERE quantity = 0
INTERSECT
SELECT product_id FROM warehouse_2 WHERE quantity = 0;
```
<img width="158" height="153" alt="image" src="https://github.com/user-attachments/assets/91954580-4f6e-4fc4-a8ad-55eaac52175e" />

 
`EXCEPT` — товары, находящиеся на складе 1, но отсутствующие на складе 2:
 
```sql
SELECT product_name FROM warehouse_1
EXCEPT
SELECT product_name FROM warehouse_2;
```
<img width="231" height="126" alt="image" src="https://github.com/user-attachments/assets/a2bcef49-cd11-4a36-bdb1-1754df8493ec" />

 
## 4. Оконные функции (Window Functions)
 
Ранжирование сотрудников по зарплате внутри каждого отдела:
 
```sql
SELECT
    first_name,
    last_name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_salary_rank,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num
FROM employees;
```
<img width="812" height="243" alt="image" src="https://github.com/user-attachments/assets/e9e971da-3cbb-40ee-8f06-db9dc6b6da4c" />

 
## 5. Транспонирование (Pivot и Unpivot)
 
Pivot с помощью условной агрегации (строки в столбцы):
 
```sql
SELECT
    product_name,
    SUM(CASE WHEN year = 2022 THEN sales_amount ELSE 0 END) AS year_2022,
    SUM(CASE WHEN year = 2023 THEN sales_amount ELSE 0 END) AS year_2023,
    SUM(CASE WHEN year = 2024 THEN sales_amount ELSE 0 END) AS year_2024
FROM sales
GROUP BY product_name;
```
<img width="481" height="179" alt="image" src="https://github.com/user-attachments/assets/041aae22-73fc-4aeb-9385-5d87262f8658" />

 
Unpivot с помощью `UNION ALL` (столбцы в строки):
 
```sql
SELECT product_name, '2022' AS year, year_2022 AS sales_amount FROM pivoted_sales
UNION ALL
SELECT product_name, '2023' AS year, year_2023 AS sales_amount FROM pivoted_sales
UNION ALL
SELECT product_name, '2024' AS year, year_2024 AS sales_amount FROM pivoted_sales
ORDER BY product_name, year;
```
<img width="381" height="270" alt="image" src="https://github.com/user-attachments/assets/1433ae43-c1e6-421c-b66a-360ca663c619" />

 
## 6. Сложная фильтрация и сортировка
 
Предложение `FILTER (WHERE ...)`:
 
```sql
SELECT
    employee_id,
    SUM(salary) OVER (PARTITION BY department) AS total_dept_salary,
    AVG(salary) FILTER (WHERE tenure > 5) OVER () AS avg_salary_senior
FROM employees;
```
<img width="427" height="241" alt="image" src="https://github.com/user-attachments/assets/7a8bdebb-1314-459b-a1ae-cf6b94fe1342" />

 
Кастомная сортировка с помощью `CASE`:
 
```sql
SELECT product_name, status
FROM order_statuses
ORDER BY
    CASE status
        WHEN 'High Priority' THEN 1
        WHEN 'Medium Priority' THEN 2
        WHEN 'Low Priority' THEN 3
        ELSE 4
    END;
```
<img width="367" height="227" alt="image" src="https://github.com/user-attachments/assets/0b03cd46-92d5-4a09-a3ff-84b491c2688e" />

 
## 7. Оптимизация запросов (EXPLAIN & EXPLAIN ANALYZE)
 
Просмотр плана выполнения запроса:
 
```sql
EXPLAIN SELECT * FROM employees WHERE salary > 80000;
```
 <img width="426" height="176" alt="image" src="https://github.com/user-attachments/assets/389a4d88-ef5e-4623-8fe7-201a448c86a1" />

Реальное выполнение с замером времени и выводом метрик:
 
```sql
EXPLAIN ANALYZE SELECT * FROM employees WHERE salary > 80000;
```
<img width="665" height="258" alt="image" src="https://github.com/user-attachments/assets/c4886087-81e6-4be1-9ca2-eba718a9a0b8" />

 
Создание индекса для ускорения поиска по фильтрующим столбцам:
 
```sql
CREATE INDEX idx_employees_salary ON employees(salary);
```
<img width="560" height="305" alt="image" src="https://github.com/user-attachments/assets/d448f43c-1f87-4a35-8f0f-50d2721bb8da" />



