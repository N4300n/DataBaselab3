# Joining Tables
 
### A. INNER JOIN
 
Возвращает только совпадающие записи из обеих таблиц.
Пример: список сотрудников, у которых зафиксированы навыки.
 
```sql
SELECT e.first_name, e.last_name, e.department, s.skill_name
FROM employees e
INNER JOIN employee_skills s ON e.employee_id = s.employee_id;
```
<img width="655" height="230" alt="image" src="https://github.com/user-attachments/assets/d1e9449c-193a-4065-8b21-65fbc24b3b85" />

 
### B. LEFT JOIN (LEFT OUTER JOIN)
 
Все записи левой таблицы + совпадающие из правой.
Пример: все товары и их отзывы (товары без отзывов выводят NULL).
 
```sql
SELECT p.product_id, p.category, p.price, r.rating
FROM products p
LEFT JOIN product_reviews r ON p.product_id = r.product_id;
```
<img width="520" height="363" alt="image" src="https://github.com/user-attachments/assets/6c9d23ef-ba57-45e5-947a-7b2ffc59d44d" />

 
### C. RIGHT JOIN (RIGHT OUTER JOIN)
 
Все записи правой таблицы + совпадающие из левой.
Пример: все отзывы с указанием категории товара.
 
```sql
SELECT p.category, p.price, r.review_id, r.rating
FROM products p
RIGHT JOIN product_reviews r ON p.product_id = r.product_id;
```
<img width="521" height="323" alt="image" src="https://github.com/user-attachments/assets/187e5c87-e550-4cb0-a96d-94e460d29c3a" />

 
### D. FULL OUTER JOIN
 
Все записи из обеих таблиц (несовпадения заполняются NULL).
Пример: полный сопоставительный список сотрудников и всех навыков.
 
```sql
SELECT e.first_name, e.last_name, s.skill_name
FROM employees e
FULL OUTER JOIN employee_skills s ON e.employee_id = s.employee_id;
```
 <img width="531" height="295" alt="image" src="https://github.com/user-attachments/assets/2c89fc93-7955-4e9b-b5c5-d72cb143167b" />

### E. CROSS JOIN
 
Декартово произведение (каждая строка первой таблицы с каждой строкой второй).
Пример: комбинация всех сотрудников и всех категорий товаров.
 
```sql
SELECT e.first_name, p.category, p.price
FROM employees e
CROSS JOIN products p;
```

<img width="471" height="344" alt="image" src="https://github.com/user-attachments/assets/1c767219-13e5-4fcd-bee0-845b74292791" />

 
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
<img width="323" height="372" alt="image" src="https://github.com/user-attachments/assets/107f93d4-618e-40ee-9c5d-df297dbd4e80" />

 
### JOIN с дополнительными условиями фильтрации (WHERE)
 
Пример: сотрудники отдела Sales с указанием их навыков и зарплатой от 60 000.
 
```sql
SELECT e.first_name, e.department, e.salary, s.skill_name
FROM employees e
INNER JOIN employee_skills s ON e.employee_id = s.employee_id
WHERE e.department = 'Sales' AND e.salary >= 60000;
```
<img width="610" height="142" alt="image" src="https://github.com/user-attachments/assets/72f259e9-6fdc-41b3-a44f-b75f178879c4" />

 
### Поиск записей с отсутствующими связями (LEFT JOIN + IS NULL)
 
Пример: найти сотрудников, у которых НЕ указаны навыки.
 
```sql
SELECT e.first_name, e.last_name, e.department
FROM employees e
LEFT JOIN employee_skills s ON e.employee_id = s.employee_id
WHERE s.skill_name IS NULL;
```
 <img width="504" height="199" alt="image" src="https://github.com/user-attachments/assets/d5bc9281-0522-41c1-b957-556e94d086f3" />

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
<img width="412" height="198" alt="image" src="https://github.com/user-attachments/assets/b565d79e-4f3b-4dbe-944e-07ef36f9eee4" />

 
## 4. Оптимизация и индексы для JOIN
 
Создание индексов на столбцы внешних ключей для ускорения работы JOIN:
 
```sql
CREATE INDEX idx_employee_skills_emp_id ON employee_skills(employee_id);
CREATE INDEX idx_product_reviews_product_id ON product_reviews(product_id);
CREATE INDEX idx_employees_manager_id ON employees(manager_id);
```
 
