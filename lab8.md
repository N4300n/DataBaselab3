# Foreign Keys and Relationships (PostgreSQL)

## 1. Способы создания внешних ключей (FOREIGN KEY)

Родительская таблица:

```sql
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);
```

<img width="396" height="318" alt="image" src="https://github.com/user-attachments/assets/fc5349cb-6a66-496b-b27e-7d3991e2e1ff" />


### Способ 1: Встроенное объявление (Inline)

```sql
CREATE TABLE employees_inline (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER REFERENCES departments(dept_id)
);
```

<img width="515" height="304" alt="image" src="https://github.com/user-attachments/assets/30ee082b-f4d7-4159-819c-814426b1f50d" />


### Способ 2: На уровне таблицы (Table-level)

```sql
CREATE TABLE employees_table_level (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

<img width="553" height="329" alt="image" src="https://github.com/user-attachments/assets/e86a6f8d-f5c0-490d-b37d-4c82083f96ae" />

### Способ 3: Именованное ограничение (Named constraint)

```sql
CREATE TABLE employees_named (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER,
    CONSTRAINT fk_employee_department FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

<img width="798" height="314" alt="image" src="https://github.com/user-attachments/assets/a0c92e76-648a-4b0f-a15e-56b545d33f75" />


### Способ 4: Добавление внешнего ключа через ALTER TABLE

```sql
CREATE TABLE employees_alter (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER
);

ALTER TABLE employees_alter
ADD CONSTRAINT fk_employee_department_alter
FOREIGN KEY (dept_id) REFERENCES departments(dept_id);
```

<img width="571" height="383" alt="image" src="https://github.com/user-attachments/assets/bea2dd6a-b3fc-4687-b43f-a515cdf19a51" />


## 2. Связь «Один к одному» (One-to-One)

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

<img width="481" height="322" alt="image" src="https://github.com/user-attachments/assets/13173fbc-78ac-4298-ac4d-bf8f7d557a4c" />


Ограничение `UNIQUE` на `user_id` гарантирует связь «Один к одному»:

```sql
CREATE TABLE user_profiles (
    profile_id SERIAL PRIMARY KEY,
    user_id INTEGER UNIQUE NOT NULL,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    bio TEXT,
    profile_picture_url VARCHAR(255),
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```
<img width="668" height="308" alt="image" src="https://github.com/user-attachments/assets/a05254df-31ac-4982-8711-c9a17b8b51d6" />


## 3. Связь «Один ко многим» (One-to-Many) и CASCADE варианты

```sql
CREATE TABLE blog_posts (
    post_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    author VARCHAR(100),
    published_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
<img width="536" height="301" alt="image" src="https://github.com/user-attachments/assets/1898b9d6-645c-404b-af71-da8e7f4d0a13" />


Связь 1-ко-многим с каскадным удалением и обновлением:

```sql
CREATE TABLE comments (
    comment_id SERIAL PRIMARY KEY,
    post_id INTEGER NOT NULL,
    commenter_name VARCHAR(100),
    comment_text TEXT NOT NULL,
    comment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES blog_posts(post_id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE
);
```
<img width="549" height="301" alt="image" src="https://github.com/user-attachments/assets/f9ed7dec-db28-43bc-9f6a-2f5123f5e629" />


## 4. Связь «Многие ко многим» (Many-to-Many)

```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    enrollment_date DATE DEFAULT CURRENT_DATE
);

CREATE TABLE courses (
    course_id SERIAL PRIMARY KEY,
    course_code VARCHAR(10) UNIQUE NOT NULL,
    course_name VARCHAR(100) NOT NULL,
    credits INTEGER NOT NULL,
    instructor VARCHAR(100)
);
```

<img width="502" height="397" alt="image" src="https://github.com/user-attachments/assets/2b31050f-f327-4b60-aef3-1164585217de" />


Промежуточная таблица связей (Junction Table):

```sql
CREATE TABLE student_enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INTEGER NOT NULL,
    course_id INTEGER NOT NULL,
    enrollment_date DATE DEFAULT CURRENT_DATE,
    grade CHAR(2),
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id) ON DELETE CASCADE,
    UNIQUE(student_id, course_id)
);
```

<img width="726" height="308" alt="image" src="https://github.com/user-attachments/assets/cefd437a-b4ab-4ee2-ad42-2b04019ee028" />


## 5. Примеры вставки данных и запросов (JOIN)

Заполнение родительских таблиц:

```sql
INSERT INTO departments (dept_name, location) VALUES
('Engineering', 'Building A'),
('Marketing', 'Building B'),
('HR', 'Building C');

INSERT INTO students (first_name, last_name, email) VALUES
('Alice', 'Johnson', 'alice@university.edu'),
('Bob', 'Smith', 'bob@university.edu'),
('Carol', 'Wilson', 'carol@university.edu');

INSERT INTO courses (course_code, course_name, credits, instructor) VALUES
('CS101', 'Introduction to Programming', 3, 'Dr. Brown'),
('CS201', 'Data Structures', 4, 'Dr. Davis'),
('MATH101', 'Calculus I', 4, 'Dr. Wilson');
```

<img width="712" height="323" alt="image" src="https://github.com/user-attachments/assets/681366f9-a836-4b43-8f38-813665009dbd" />
<img width="463" height="191" alt="image" src="https://github.com/user-attachments/assets/992c283e-f1df-431d-aabb-2ab2047fcf29" />
<img width="721" height="186" alt="image" src="https://github.com/user-attachments/assets/3cebdc8b-fe8c-47a3-aa9e-b930e6b1079f" />
<img width="697" height="202" alt="image" src="https://github.com/user-attachments/assets/55cfcf03-44dc-4155-b77a-e3f0efafc9b5" />



Запись на курсы (Many-to-Many):

```sql
INSERT INTO student_enrollments (student_id, course_id, grade) VALUES
(1, 1, 'A'),   -- Alice -> CS101
(1, 2, 'B+'),  -- Alice -> CS201
(2, 1, 'A-'),  -- Bob -> CS101
(2, 3, 'B'),   -- Bob -> MATH101
(3, 2, 'A'),   -- Carol -> CS201
(3, 3, 'A-');  -- Carol -> MATH101
```
<img width="642" height="220" alt="image" src="https://github.com/user-attachments/assets/d3f1dac4-da2d-4e13-a388-dd2db7a7e973" />
<img width="553" height="265" alt="image" src="https://github.com/user-attachments/assets/6421f2c7-dc48-4c5d-91fc-265f67a3c1d1" />


Запрос с JOIN для отображения связанных данных:

```sql
SELECT 
    s.first_name || ' ' || s.last_name AS student_name,
    c.course_code,
    c.course_name,
    se.grade,
    se.enrollment_date
FROM students s
JOIN student_enrollments se ON s.student_id = se.student_id
JOIN courses c ON se.course_id = c.course_id
ORDER BY s.last_name, c.course_code;
```
<img width="703" height="274" alt="image" src="https://github.com/user-attachments/assets/cb5e741e-a178-453f-bc98-0ff6ae328bab" />

