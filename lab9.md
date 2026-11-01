# Database Design Basics
## 1. Примеры нормализации данных (1NF, 2NF, 3NF)
 
### Первая нормальная форма (1NF)
 
Ошибка (Violating 1NF): хранение нескольких значений в одном столбце:
 
```sql
CREATE TABLE students_1nf_bad (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    phone_numbers TEXT -- Нарушение: некорректно хранить список номеров списком
);
```
<img width="725" height="370" alt="image" src="https://github.com/user-attachments/assets/5f5e83df-bc60-41d9-834b-6e72d050df85" />

 
Решение (1NF): атомарные значения, вынос номеров в отдельную таблицу:
 
```sql
CREATE TABLE students_1nf_good (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);
 
CREATE TABLE student_phones (
    student_id INT,
    phone_number VARCHAR(15),
    phone_type VARCHAR(20), -- 'mobile', 'home' и т.д.
    PRIMARY KEY (student_id, phone_number),
    FOREIGN KEY (student_id) REFERENCES students_1nf_good(student_id)
);
```

<img width="638" height="345" alt="image" src="https://github.com/user-attachments/assets/cb8a4dda-d2dc-497d-8056-3487eb714565" />

 
### Вторая нормальная форма (2NF)
 
Ошибка (Violating 2NF): частичная зависимость от составного первичного ключа:
 
```sql
CREATE TABLE enrollments_2nf_bad (
    student_id INT,
    course_id INT,
    course_name VARCHAR(100), -- Нарушение: зависит только от course_id, а не от всего ключа
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id)
);
```
<img width="773" height="365" alt="image" src="https://github.com/user-attachments/assets/30b45420-37da-4d25-aeb9-7c86f728c6be" />

 
Решение (2NF): разделение на отдельную таблицу курсов и таблицу связей:
 
```sql
CREATE TABLE courses_2nf_good (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    credits INT
);
 
CREATE TABLE enrollments_2nf_good (
    student_id INT,
    course_id INT,
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students_1nf_good(student_id),
    FOREIGN KEY (course_id) REFERENCES courses_2nf_good(course_id)
);
```
<img width="656" height="307" alt="image" src="https://github.com/user-attachments/assets/2f190206-95b4-4bd5-91fc-5eeb6eb5994f" />

 
### Третья нормальная форма (3NF)
 
Ошибка (Violating 3NF): транзитивная зависимость неключевых атрибутов:
 
```sql
CREATE TABLE students_3nf_bad (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    department_name VARCHAR(100) -- Нарушение: зависит от department_id, а не от student_id
);
```
<img width="768" height="339" alt="image" src="https://github.com/user-attachments/assets/0e574f77-10cc-4060-8134-51ae4e983a50" />

 
Решение (3NF): вынос информации о факультете в отдельную таблицу:
 
```sql
CREATE TABLE departments_3nf_good (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100),
    department_head VARCHAR(100)
);
 
CREATE TABLE students_3nf_good (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments_3nf_good(department_id)
);
```
<img width="714" height="309" alt="image" src="https://github.com/user-attachments/assets/828cbd1a-7a50-4c20-95db-baa8d3e1652f" />

 
## 2. Система управления библиотекой (Library Management System)
 
Авторы книг:
 
```sql
CREATE TABLE authors (
    author_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE
);
```
<img width="389" height="252" alt="image" src="https://github.com/user-attachments/assets/aaddfd28-cf34-4149-87e3-2e17b25257c1" />

Книги:
 
```sql
CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    isbn VARCHAR(13) UNIQUE,
    publication_year INTEGER,
    available_copies INTEGER DEFAULT 1
);
```
<img width="391" height="341" alt="image" src="https://github.com/user-attachments/assets/8b6944c4-49a5-4b6a-b27f-5741bdd21e44" />

 
Связывающая таблица для связи «Многие-ко-многим» (Книги <-> Авторы):
 
```sql
CREATE TABLE book_authors (
    book_id INTEGER,
    author_id INTEGER,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES authors(author_id) ON DELETE CASCADE
);
```
<img width="735" height="301" alt="image" src="https://github.com/user-attachments/assets/22ba5695-cd0b-4678-9f5e-e2eafc902fef" />

 
Читатели:
 
```sql
CREATE TABLE members (
    member_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(15),
    membership_date DATE DEFAULT CURRENT_DATE
);
```
<img width="445" height="332" alt="image" src="https://github.com/user-attachments/assets/6dc90c07-9ac9-411d-94e3-a166de8c01d3" />

Выдача книг (Loans):
 
```sql
CREATE TABLE loans (
    loan_id SERIAL PRIMARY KEY,
    member_id INTEGER NOT NULL,
    book_id INTEGER NOT NULL,
    loan_date DATE DEFAULT CURRENT_DATE,
    due_date DATE NOT NULL,
    return_date DATE,
    late_fee DECIMAL(10, 2) DEFAULT 0.00,
    FOREIGN KEY (member_id) REFERENCES members(member_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id)
);
```
<img width="555" height="289" alt="image" src="https://github.com/user-attachments/assets/8bee07b7-d4ce-418a-a2d8-9f229caa1d4c" />

 
## 3. Примеры вставки данных и сложных запросов
 
Вставка данных:
 
```sql
INSERT INTO authors (first_name, last_name, birth_date) VALUES 
('George', 'Orwell', '1903-06-25'),
('J.K.', 'Rowling', '1965-07-31');
 
INSERT INTO books (title, isbn, publication_year, available_copies) VALUES 
('1984', '9780451524935', 1949, 5),
('Animal Farm', '9780451526342', 1945, 3);
 
INSERT INTO book_authors (book_id, author_id) VALUES 
(1, 1),
(2, 1);
 
INSERT INTO members (first_name, last_name, email, phone) VALUES 
('John', 'Doe', 'john.doe@example.com', '555-0199');
 
INSERT INTO loans (member_id, book_id, due_date) VALUES 
(1, 1, CURRENT_DATE + INTERVAL '14 days');
```
<img width="693" height="370" alt="image" src="https://github.com/user-attachments/assets/cb135e5d-2cb9-45da-90b2-2b9f4dcf44c5" />

Запрос для получения выданных книг с информацией о читателе и авторе:
 
```sql
SELECT 
    m.first_name || ' ' || m.last_name AS member_name,
    b.title AS book_title,
    a.first_name || ' ' || a.last_name AS author_name,
    l.loan_date,
    l.due_date
FROM loans l
JOIN members m ON l.member_id = m.member_id
JOIN books b ON l.book_id = b.book_id
JOIN book_authors ba ON b.book_id = ba.book_id
JOIN authors a ON ba.author_id = a.author_id;
```
<img width="593" height="167" alt="image" src="https://github.com/user-attachments/assets/e8e4eb74-e586-4f88-924f-cb888cbd2253" />

