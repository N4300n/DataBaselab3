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

### Способ 1: Встроенное объявление (Inline)

```sql
CREATE TABLE employees_inline (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER REFERENCES departments(dept_id)
);
```

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

## 2. Связь «Один к одному» (One-to-One)

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

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
