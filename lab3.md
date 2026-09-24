# Практика работы с PostgreSQL (psql)

В этом репозитории собраны базовые команды и SQL-запросы для работы с PostgreSQL через интерактивный терминал `psql`. Практика выполнялась в среде Windows 11.

## 1. Подключение к базе данных

Для подключения к серверу PostgreSQL локально используется следующая команда в терминале (`cmd` или PowerShell):

```bash
psql -U postgres -h localhost -p 5432
```
*Где `-U` — имя пользователя, `-h` — хост, `-p` — порт. Чтобы сразу подключиться к конкретной БД, можно добавить `-d имя_базы`.*

<img width="726" height="257" alt="image" src="https://github.com/user-attachments/assets/eb8df8dd-f7dc-4cd7-a872-75fc124f31c1" />


## 2. Основные мета-команды psql

Эти команды используются внутри терминала `psql` для управления и навигации (начинаются с обратного слеша `\`):

* `\l` — вывести список всех баз данных на сервере
* `\c <dbname>` — подключиться к конкретной базе данных (например, `\c mydb`)
* `\dt` — показать список всех таблиц в текущей подключенной базе
* `\d <table>` — показать подробную структуру конкретной таблицы (колонки, типы данных, ключи)
* `\q` — выйти из интерактивного терминала `psql`
  
<img width="1309" height="289" alt="image" src="https://github.com/user-attachments/assets/e99e119a-40a8-4fc5-87cc-243bc53efb60" />

<img width="767" height="395" alt="image" src="https://github.com/user-attachments/assets/01cff795-6ae0-4cb3-9f05-bc49d3b6eb59" />

## 3. Базовые SQL-операции (CRUD)

Примеры SQL-запросов, выполненных в рамках практики прямо в терминале `psql`:

### Создание таблицы (Create)
```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
```
<img width="847" height="33" alt="image" src="https://github.com/user-attachments/assets/84c4c61a-2daf-4871-85f3-ba9881db4059" />

### Добавление данных (Insert)
```sql
INSERT INTO students (name, age) 
VALUES 
    ('Alice', 21), 
    ('Bob', 23);
```
<img width="795" height="119" alt="image" src="https://github.com/user-attachments/assets/a1769008-d20f-4672-addf-d5bc5acd3ae9" />


### Чтение данных (Select)
```sql
SELECT * FROM students;
```
<img width="333" height="177" alt="image" src="https://github.com/user-attachments/assets/a29a2f13-40e5-4e47-a8e3-7c29f2a5e8fc" />

### Удаление таблицы (Drop)
```sql
DROP TABLE students;
```
<img width="969" height="111" alt="image" src="https://github.com/user-attachments/assets/c5d3c2f7-8936-4a86-b9f6-61a81a0c5730" />
