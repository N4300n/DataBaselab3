# Joining Tables
 
### A. INNER JOIN
 
Возвращает только совпадающие записи из обеих таблиц.
Пример: список сотрудников, у которых зафиксированы навыки.
 
```sql
SELECT e.first_name, e.last_name, e.department, s.skill_name
FROM employees e
INNER JOIN employee_skills s ON e.employee_id = s.employee_id;
```
 
### B. LEFT JOIN (LEFT OUTER JOIN)
 
Все записи левой таблицы + совпадающие из правой.
Пример: все товары и их отзывы (товары без отзывов выводят NULL).
 
```sql
SELECT p.product_id, p.category, p.price, r.rating
FROM products p
LEFT JOIN product_reviews r ON p.product_id = r.product_id;
```
 
### C. RIGHT JOIN (RIGHT OUTER JOIN)
 
Все записи правой таблицы + совпадающие из левой.
Пример: все отзывы с указанием категории товара.
 
```sql
SELECT p.category, p.price, r.review_id, r.rating
FROM products p
RIGHT JOIN product_reviews r ON p.product_id = r.product_id;
```
 
### D. FULL OUTER JOIN
 
Все записи из обеих таблиц (несовпадения заполняются NULL).
Пример: полный сопоставительный список сотрудников и всех навыков.
 
```sql
SELECT e.first_name, e.last_name, s.skill_name
FROM employees e
FULL OUTER JOIN employee_skills s ON e.employee_id = s.employee_id;
```
 
### E. CROSS JOIN
 
Декартово произведение (каждая строка первой таблицы с каждой строкой второй).
Пример: комбинация всех сотрудников и всех категорий товаров.
 
```sql
SELECT e.first_name, p.category, p.price
FROM employees e
CROSS JOIN products p;
```
 
## 3. Продвинутые техники и соединения
 
### Self Join (соединение таблицы самой с собой)
 
Пример: список сотрудников и имён их менеджеров.
 
```sql
SELECT 
    e.first_name || ' ' || e.last_name AS employee,
    m.first_name || ' ' || m.last_name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;
```
 
### JOIN с дополнительными условиями фильтрации (WHERE)
 
Пример: сотрудники отдела Sales с указанием их навыков и зарплатой от 60 000.
 
```sql
SELECT e.first_name, e.department, e.salary, s.skill_name
FROM employees e
INNER JOIN employee_skills s ON e.employee_id = s.employee_id
WHERE e.department = 'Sales' AND e.salary >= 60000;
```
 
### Поиск записей с отсутствующими связями (LEFT JOIN + IS NULL)
 
Пример: найти сотрудников, у которых НЕ указаны навыки.
 
```sql
SELECT e.first_name, e.last_name, e.department
FROM employees e
LEFT JOIN employee_skills s ON e.employee_id = s.employee_id
WHERE s.skill_name IS NULL;
```
 
### Агрегация данных с использованием JOIN
 
Пример: средняя оценка отзывов для каждой категории товаров.
 
```sql
SELECT 
    p.category, 
    COUNT(r.review_id) AS total_reviews,
    ROUND(AVG(r.rating), 2) AS avg_rating
FROM products p
LEFT JOIN product_reviews r ON p.product_id = r.product_id
GROUP BY p.category;
```
 
## 4. Оптимизация и индексы для JOIN
 
Создание индексов на столбцы внешних ключей для ускорения работы JOIN:
 
```sql
CREATE INDEX idx_employee_skills_emp_id ON employee_skills(employee_id);
CREATE INDEX idx_product_reviews_product_id ON product_reviews(product_id);
CREATE INDEX idx_employees_manager_id ON employees(manager_id);
```
 
