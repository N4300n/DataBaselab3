# Viewing Database and Table Structure
### 1. Таблица поставщиков (Suppliers)
 
```sql
CREATE TABLE suppliers (
    sup_id SERIAL PRIMARY KEY,
    sup_name VARCHAR(255),
    street VARCHAR(255),
    city VARCHAR(255),
    state VARCHAR(25),
    zip VARCHAR(255)
);
```
<img width="332" height="202" alt="image" src="https://github.com/user-attachments/assets/92f42d68-9e3f-474f-92cc-5c333d6f9324" />

 
### 2. Таблица кофейных заведений (Coffee Houses)
 
```sql
CREATE TABLE coffee_houses (
    store_id INT PRIMARY KEY NOT NULL,
    coffee INT,
    merch INT,
    total INT,
    city VARCHAR(255)
);
```

<img width="396" height="234" alt="image" src="https://github.com/user-attachments/assets/d713e0e6-0fd1-4ec7-97fb-61b214727ac0" />

 
### 3. Таблица сортов кофе (Coffees)
 
```sql
CREATE TABLE coffees (
    cof_name VARCHAR(32) PRIMARY KEY,
    sup_id INT REFERENCES suppliers(sup_id) ON DELETE SET NULL,
    price REAL,
    sales INT,
    total INT
);
```
<img width="602" height="195" alt="image" src="https://github.com/user-attachments/assets/ea6aa081-8a69-4164-b626-fbc26d6a48bd" />

 
### 4. Складской учет кофе (Coffee Inventory)
 
```sql
CREATE TABLE cof_inventory (
    warehouse_id INT PRIMARY KEY,
    cof_name VARCHAR(32) REFERENCES coffees(cof_name) ON DELETE CASCADE,
    sup_id INT REFERENCES suppliers(sup_id) ON DELETE CASCADE,
    date_val DATE,
    quan INT
);
```
<img width="667" height="239" alt="image" src="https://github.com/user-attachments/assets/f3618d14-cafa-402d-b668-b30f5ee5c2a1" />

 
### 5. Инвентарь товаров/мерча (Merch Inventory)
 
```sql
CREATE TABLE merch_inventory (
    item_id INT PRIMARY KEY,
    item_name VARCHAR(255),
    sup_id INT REFERENCES suppliers(sup_id) ON DELETE CASCADE,
    date DATE,
    quan INT
);
```
<img width="595" height="239" alt="image" src="https://github.com/user-attachments/assets/9e78ed7b-4a00-4bdf-a18a-b838ddc10e17" />

### 6. Таблица студентов (University Students)
 
```sql
CREATE TABLE university_students (
    student_id SERIAL PRIMARY KEY,
    first_name TEXT,
    last_name VARCHAR(50),
    email_address VARCHAR(100),
    date_of_birth DATE
);
```

<img width="364" height="227" alt="image" src="https://github.com/user-attachments/assets/77e761ff-c59e-453d-aba5-c79a0d280e32" />

## Тестовые данные для проверки
 
```sql
INSERT INTO suppliers (sup_name, street, city, state, zip) VALUES
('Acme Coffee Co.', '123 Main St', 'Seattle', 'WA', '98101'),
('Bean Imports', '456 Market St', 'Portland', 'OR', '97201');
 
INSERT INTO coffee_houses (store_id, coffee, merch, total, city) VALUES
(101, 50, 20, 70, 'Seattle'),
(102, 30, 15, 45, 'Portland');
 
INSERT INTO coffees (cof_name, sup_id, price, sales, total) VALUES
('Arabica Premium', 1, 15.50, 100, 1500),
('Robusta Dark', 2, 12.00, 80, 960);
 
INSERT INTO university_students (first_name, last_name, email_address, date_of_birth) VALUES
('John', 'Doe', 'john.doe@university.edu', '2001-05-15');
```
<img width="868" height="327" alt="image" src="https://github.com/user-attachments/assets/0a75cb0c-299a-4d0d-a94e-a0bc70e25860" />

<img width="1098" height="941" alt="image" src="https://github.com/user-attachments/assets/953e7333-6b6c-4ecd-8e9f-1294da4d5648" />
