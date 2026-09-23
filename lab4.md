# База данных: Аренда машин 🚗

Здесь я создал таблицу для учета машин в PostgreSQL.

## Мой SQL-код:

```sql
CREATE TABLE cars (
    car_id SERIAL PRIMARY KEY,
    brand VARCHAR(50),
    model VARCHAR(50),
    price_per_day INT,
    status VARCHAR(20)
);

INSERT INTO cars (brand, model, price_per_day, status) VALUES
('Toyota', 'Camry', 50, 'available'),
('Hyundai', 'Elantra', 40, 'rented'),
('Kia', 'Rio', 35, 'available'),
('Mercedes', 'S-Class', 150, 'available'),
('BMW', 'X5', 120, 'rented');
```

## Как это выглядит (Скриншот):
<img width="1089" height="706" alt="image" src="https://github.com/user-attachments/assets/3a10a9f1-9d4f-44c6-9fe1-8a8fe5920f19" />

1. Структура SELECT и вывод всех данных

```sql
SELECT * FROM cars;
```
<img width="709" height="448" alt="image" src="https://github.com/user-attachments/assets/3f339a76-2fa3-4059-ae4b-82a8dc7641f0" />


2. Выбор конкретных столбцов

```sql
SELECT brand, model FROM cars;
```
<img width="459" height="431" alt="image" src="https://github.com/user-attachments/assets/4dcc4039-677d-472a-9e72-62c38699a69f" />


3. Базовый фильтр WHERE

```sql
SELECT brand, model, status 
FROM cars 
WHERE status = 'available';
```
<img width="667" height="508" alt="image" src="https://github.com/user-attachments/assets/1fabf6cd-05a6-4f5d-979e-e490aab24716" />


4. Сортировка с помощью ORDER BY
Отсортируем машины по цене аренды в день:

```sql
SELECT brand, model, price_per_day 
FROM cars 
ORDER BY price_per_day;
```
<img width="500" height="472" alt="image" src="https://github.com/user-attachments/assets/6f55c62f-4083-43ee-9968-44560db5b435" />


5. Ограничение вывода с помощью LIMIT

```sql
SELECT brand, model 
FROM cars 
LIMIT 2;
```
<img width="422" height="390" alt="image" src="https://github.com/user-attachments/assets/172a1683-61e9-4954-b1d2-805d6222ecdd" />


6. Использование комментариев
   
```sql
-- Этот запрос ищет доступные машины дешевле 60 долларов[cite: 1]
SELECT brand, model, price_per_day 
FROM cars 
WHERE status = 'available' AND price_per_day < 60;
```

<img width="532" height="431" alt="image" src="https://github.com/user-attachments/assets/a2ed4685-279d-480e-b425-2aaf8e9a0f25" />


