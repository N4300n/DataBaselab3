# Basic Data Operations

### INSERT Statements
The INSERT statement is used to add new rows into a table.
---
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```
### Example : 
```sql
CREATE TABLE Students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    birth_date DATE
);
```
<img width="343" height="166" alt="image" src="https://github.com/user-attachments/assets/128c9338-cb64-4998-9b15-4e7a753df591" />

### Insert a Students : 
```sql
INSERT INTO Students (first_name, last_name, birth_date)
VALUES ('Aibek', 'Sharshenov', '2002-05-12');
```
<img width="532" height="311" alt="image" src="https://github.com/user-attachments/assets/9aba0f5b-da88-4928-8ba7-c281841ceb77" />
<img width="532" height="151" alt="image" src="https://github.com/user-attachments/assets/eb2bc6d7-b856-4724-b3d2-6509fc38a154" />

### Inserting multiple records at once : 
```sql
INSERT INTO Students (first_name, last_name, birth_date) VALUES
('Ainura', 'Toktomamatova', '2001-09-23'),
('Bakyt', 'Mamatov', '2003-01-15'),
('Gulzat', 'Sultanova', '2002-07-30');
```
<img width="591" height="306" alt="image" src="https://github.com/user-attachments/assets/76667632-7359-4f73-a793-d9d9eeb0c3cf" />
<img width="536" height="228" alt="image" src="https://github.com/user-attachments/assets/cb8ee0d6-0546-445d-b07e-108c81f3dc01" />

# UPDATE Statements
### The UPDATE statement modifies existing records.
It must usually be combined with a WHERE clause to avoid changing all rows.
### Syntax:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

<img width="541" height="101" alt="image" src="https://github.com/user-attachments/assets/13e437f2-d664-4c8e-9b5e-c1b57827ee0c" />
<img width="531" height="220" alt="image" src="https://github.com/user-attachments/assets/1a4ddf60-e91c-4187-a1e5-850458072d57" />

### Example : 
```sql
UPDATE Students
SET birth_date = '2002-05-15'
WHERE first_name = 'Aibek' AND last_name = 'Sharshenov';
```
<img width="566" height="104" alt="image" src="https://github.com/user-attachments/assets/42c57cdd-509b-4960-9617-30e07cc1fa5d" />
<img width="545" height="223" alt="image" src="https://github.com/user-attachments/assets/8b24db2c-b866-410e-9c13-5e24acb44441" />

# DELETE Statements
## The DELETE statement removes rows from a table.
### Syntax :  
```sql
DELETE FROM table_name
WHERE condition;
```
#### Important: Omitting WHERE will delete all rows in the table.

### Example:
```sql
DELETE FROM Students
WHERE first_name = 'Gulzat' AND last_name = 'Sultanova';
``` 
<img width="337" height="99" alt="image" src="https://github.com/user-attachments/assets/7c3997db-d899-4b24-bae5-65a2018b34c3" />

<img width="549" height="183" alt="image" src="https://github.com/user-attachments/assets/48dfc4a9-5a55-4300-8910-df21fef0b6a3" />
<img width="389" height="124" alt="image" src="https://github.com/user-attachments/assets/f0ee82e3-8527-46fd-8cb2-0186e921d609" />
<img width="534" height="169" alt="image" src="https://github.com/user-attachments/assets/afd14bfc-2d30-4e5f-a4fb-4f4d9932e5dc" />
<img width="526" height="135" alt="image" src="https://github.com/user-attachments/assets/ad370cc8-9793-4a66-9f9c-263d3b68eaa3" />
