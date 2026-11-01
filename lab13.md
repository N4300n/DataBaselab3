# 13 Aggregate Functions  
 
## 1. Базовые агрегатные функции
 
### Подсчёт количества (COUNT)
 
```sql
SELECT COUNT(*) AS total_employees FROM employees;
```
 <img width="493" height="647" alt="image" src="https://github.com/user-attachments/assets/bb7ca905-b511-45cc-bb53-0b38e0117ed6" />


```sql
SELECT COUNT(email) AS employees_with_email FROM employees;
```
 
```sql
SELECT COUNT(DISTINCT department) AS unique_departments FROM employees;
```
<img width="562" height="652" alt="image" src="https://github.com/user-attachments/assets/99305607-ff3b-45b2-9ef6-ae758b4354a9" />

 
### Суммирование (SUM)
 
```sql
SELECT SUM(salary) AS total_salaries FROM employees;
```
<img width="505" height="645" alt="image" src="https://github.com/user-attachments/assets/7ae2e42f-91f4-4465-9206-3e50023bf312" />

 
```sql
SELECT department, SUM(sales_amount) AS total_sales
FROM sales_data
GROUP BY department;
```

<img width="506" height="677" alt="image" src="https://github.com/user-attachments/assets/cda905a3-c47f-4caf-b8bc-341e6e958554" />

 
### Среднее значение (AVG)
 
```sql
SELECT AVG(salary) AS average_salary FROM employees;
```
<img width="348" height="147" alt="image" src="https://github.com/user-attachments/assets/4ecb6829-b4b0-4754-b7bd-dd9144daaa86" />

 
```sql
SELECT department, AVG(age) AS avg_age
FROM employees
GROUP BY department;
```
 
### Максимум и минимум (MAX / MIN)
 
```sql
SELECT 
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```
<img width="382" height="209" alt="image" src="https://github.com/user-attachments/assets/1dd5c817-25aa-4b5a-aca4-249d86f78245" />

 
```sql
SELECT department, MAX(hire_date) AS latest_hire
FROM employees
GROUP BY department;
```
<img width="342" height="187" alt="image" src="https://github.com/user-attachments/assets/09d3454b-ed56-4ef0-bc21-dc960818eb48" />

 
## 3. Продвинутые и статистические функции
 
### Объединение строк (STRING_AGG)
 
```sql
SELECT department, STRING_AGG(first_name, ', ') AS employee_names
FROM employees
GROUP BY department;
```
<img width="343" height="202" alt="image" src="https://github.com/user-attachments/assets/440f96bb-cdc3-48c6-be43-c04c77fc163e" />

 
```sql
SELECT employee_id, STRING_AGG(skill_name, ', ' ORDER BY skill_name) AS skills
FROM employee_skills
GROUP BY employee_id;
```
<img width="338" height="184" alt="image" src="https://github.com/user-attachments/assets/bbf78e36-f794-45dc-b5d9-6e7d6cc15c95" />

 
### Создание массивов (ARRAY_AGG)
 
```sql
SELECT department, ARRAY_AGG(salary) AS salary_array
FROM employees
GROUP BY department;
```
 <img width="390" height="211" alt="image" src="https://github.com/user-attachments/assets/3229182b-ebb3-4f5a-9396-0025cc1d5d19" />

```sql
SELECT department, ARRAY_AGG(first_name ORDER BY last_name) AS employees
FROM employees
GROUP BY department;
```
<img width="368" height="222" alt="image" src="https://github.com/user-attachments/assets/6bfd9fd8-5a72-400d-9e1e-c83f99679069" />

 
### Статистические функции (STDDEV, VARIANCE, CORR)
 
```sql
SELECT 
    department,
    STDDEV(salary) AS salary_std_dev,
    VARIANCE(salary) AS salary_variance
FROM employees
GROUP BY department;
```
<img width="491" height="705" alt="image" src="https://github.com/user-attachments/assets/1f21a649-f28b-435c-941c-ba1e46567547" />

 
```sql
SELECT CORR(experience_years, salary) AS experience_salary_correlation
FROM employees;
```
<img width="659" height="676" alt="image" src="https://github.com/user-attachments/assets/01ed314e-e420-4701-8fe9-e164a1b9dc5a" />

 
## 4. Работа с GROUP BY и HAVING
 
### Базовая группировка
 
```sql
SELECT 
    category,
    COUNT(*) AS number_of_products,
    SUM(price) AS total_value,
    AVG(price) AS average_price
FROM products
GROUP BY category;
```
<img width="594" height="170" alt="image" src="https://github.com/user-attachments/assets/3f104a86-0283-49ba-a508-61cc8c0da26c" />

 
### Группировка по нескольким столбцам / выражениям даты
 
```sql
SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(QUARTER FROM sale_date) AS quarter,
    COUNT(*) AS total_sales,
    SUM(sales_amount) AS total_revenue
FROM sales_data
GROUP BY EXTRACT(YEAR FROM sale_date), EXTRACT(QUARTER FROM sale_date)
ORDER BY year, quarter;
```
 <img width="421" height="173" alt="image" src="https://github.com/user-attachments/assets/2532d243-f348-48f3-ae26-19897e932348" />

### Группировка по вычисляемым выражениям (CASE)
 
```sql
SELECT 
    CASE 
        WHEN age < 30 THEN 'Under 30'
        WHEN age BETWEEN 30 AND 50 THEN '30-50'
        ELSE 'Over 50'
    END AS age_group,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY 
    CASE 
        WHEN age < 30 THEN 'Under 30'
        WHEN age BETWEEN 30 AND 50 THEN '30-50'
        ELSE 'Over 50'
    END;
```
<img width="417" height="199" alt="image" src="https://github.com/user-attachments/assets/3276d1c4-96d7-4eab-a6b1-e57187bc99ce" />

 
### Фильтрация групп с помощью HAVING
 
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) >= 2;
```
<img width="350" height="168" alt="image" src="https://github.com/user-attachments/assets/c2b6795b-2b5b-4954-8e2c-24a71fd52443" />

 
```sql
SELECT 
    product_id,
    AVG(rating) AS avg_rating,
    COUNT(*) AS review_count
FROM product_reviews
GROUP BY product_id
HAVING AVG(rating) > 4.0 AND COUNT(*) >= 10;
```
 <img width="401" height="176" alt="image" src="https://github.com/user-attachments/assets/6a7ef8fc-682b-40a3-acfa-6a8f164fa565" />

## 5. Оконные функции и обработка NULL
 
### Оконная функция (сохраняет детализацию строк)
 
```sql
SELECT 
    employee_id,
    first_name,
    department,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg_salary
FROM employees;
```
<img width="727" height="273" alt="image" src="https://github.com/user-attachments/assets/fee7dd21-e5af-4d93-b2a0-20ae1a3cb8ec" />

 
### Обработка NULL значений
 
```sql
SELECT 
    COUNT(*) AS total_rows,
    COUNT(email) AS non_null_emails,
    COUNT(*) - COUNT(email) AS null_emails
FROM employees;
```
<img width="375" height="158" alt="image" src="https://github.com/user-attachments/assets/683c7394-4813-4ddd-8990-93f3d2eb4223" />

 
```sql
SELECT 
    SUM(bonus) AS total_bonus,
    AVG(bonus) AS avg_bonus,
    COUNT(bonus) AS employees_with_bonus
FROM employees;
```
 <img width="480" height="155" alt="image" src="https://github.com/user-attachments/assets/da5579e0-4d0e-4a59-bdd1-33d945fc61a2" />

## 6. Сложные шаблоны (условная агрегация и проценты)
 
### Условная агрегация
 
```sql
SELECT 
    department,
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN salary > 70000 THEN 1 END) AS high_earners,
    COUNT(CASE WHEN hire_date > '2022-01-01' THEN 1 END) AS recent_hires
FROM employees
GROUP BY department;
```
 <img width="523" height="195" alt="image" src="https://github.com/user-attachments/assets/acf6f330-2a8a-4a9c-8b26-53ca101e0df2" />

### Расчёт процентной доли от общего количества
 
```sql
SELECT 
    department,
    COUNT(*) AS dept_count,
    ROUND((COUNT(*)::NUMERIC / (SELECT COUNT(*) FROM employees) * 100), 2) AS percentage
FROM employees
GROUP BY department
ORDER BY percentage DESC;
```
<img width="423" height="198" alt="image" src="https://github.com/user-attachments/assets/36c24de4-62cd-4c64-b3ea-ad4dbccc5ebc" />

