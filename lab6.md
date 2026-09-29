# Таблицы, типы данных и ограничения
### 1. CREATE TABLE (Создание таблицы)

Команда CREATE TABLE используется для создания новой таблицы. В ней мы определяем названия колонок, типы данных и ограничения (constraints).
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    faculty VARCHAR(100)
);
```
#### Использованные типы данных и ограничения:

  -SERIAL: Автоматически увеличивающееся целое число. Отлично подходит для идентификаторов.
  
  -VARCHAR(n): Строка переменной длины с лимитом n символов.
  
  -INTEGER: Целочисленный тип данных.
  
  -PRIMARY KEY: Уникально идентифицирует каждую запись в таблице.
  
  -NOT NULL: Гарантирует, что колонка не может быть пустой.
  
  -UNIQUE: Гарантирует, что все значения в колонке уникальны.
  
  -CHECK: Проверяет выполнение определенного условия (например, цена больше нуля).

<img width="427" height="365" alt="image" src="https://github.com/user-attachments/assets/5699527a-9632-4e57-8fc3-9884bb4de464" />

<img width="745" height="125" alt="image" src="https://github.com/user-attachments/assets/21a31e77-804a-46cf-a63c-bbe44bb11bb8" />

### 2. DROP TABLE (Удаление таблицы)

Команда DROP TABLE безвозвратно удаляет таблицу и все данные внутри неё. Использование конструкции IF EXISTS защищает от появления ошибки в случае, если таблицы с таким названием в базе не существует.

```sql
DROP TABLE table_name;
```

<img width="357" height="373" alt="image" src="https://github.com/user-attachments/assets/479f192f-d90d-4566-ad85-59d8cbbea212" />

Чтобы избежать ошибок на случай, если таблица может не существовать, используйте IF EXISTS:

```sql
DROP TABLE IF EXISTS test_table;
```

<img width="488" height="393" alt="image" src="https://github.com/user-attachments/assets/c37423f9-1149-4516-8fa3-86589b2ebe65" />

### 3. ALTER TABLE (Изменение структуры таблицы)

Команда ALTER TABLE позволяет модифицировать структуру уже существующей таблицы, не удаляя из неё данные.

Мы использовали несколько вариантов этой команды:

### Добавление новой колонки (ADD COLUMN) :

```sql
ALTER TABLE students 
ADD COLUMN date_of_brith DATE;
```

<img width="383" height="366" alt="image" src="https://github.com/user-attachments/assets/b2b117f3-bab7-462a-a009-f610ef37a51e" />

<img width="860" height="134" alt="image" src="https://github.com/user-attachments/assets/90834da4-879a-4453-952b-64c68ff746d9" />

### Изменение типа данных колонки (ALTER COLUMN ... TYPE) : 

Примечание: В примере ниже мы изменили тип VARCHAR на TEXT (строка без лимита длины).

```sql
ALTER TABLE car
ALTER COLUMN brand TYPE TEXT;
```
<img width="324" height="363" alt="image" src="https://github.com/user-attachments/assets/0af557fb-ed56-4b51-bb49-84ebbc01a171" />

<img width="695" height="112" alt="image" src="https://github.com/user-attachments/assets/fcdd49cc-90de-4bf8-bd93-79c0cefbf34b" />

### Переименование колонки (RENAME COLUMN) :

```sql
ALTER TABLE car
RENAME COLUMN license_plate TO registration_number;
```

<img width="359" height="343" alt="image" src="https://github.com/user-attachments/assets/949c56ef-56fb-49bf-9812-95f39ce1f340" />

<img width="632" height="114" alt="image" src="https://github.com/user-attachments/assets/72685357-1edb-495e-9ade-4d816c10490d" />

<img width="498" height="347" alt="image" src="https://github.com/user-attachments/assets/9f25fda6-5dee-4609-b156-d4115fe5c66f" />

<img width="631" height="111" alt="image" src="https://github.com/user-attachments/assets/7c5e26a0-1b5d-4ddf-a979-d697cfdf54e1" />

<img width="428" height="348" alt="image" src="https://github.com/user-attachments/assets/472a1e64-9c87-41b2-907f-2151ff24be08" />

<img width="349" height="406" alt="image" src="https://github.com/user-attachments/assets/4f1322c5-5a2d-446c-8d07-3f4f43d6f5dc" />

<img width="650" height="619" alt="image" src="https://github.com/user-attachments/assets/09008ed6-da91-4606-84e5-c632b89c071e" />

