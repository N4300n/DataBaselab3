# Transactions and ACID Properties

## 2. Основные команды транзакций (BEGIN, COMMIT, ROLLBACK)
 
Пример 1: перевод $500 со счёта 1 на счёт 2 с сохранением (`COMMIT`):
 
```sql
BEGIN;
SELECT balance FROM accounts WHERE account_id = 1;
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
COMMIT;
```
<img width="620" height="143" alt="image" src="https://github.com/user-attachments/assets/75f2257d-d992-4161-8343-99f75b89a4f2" />

 
Пример 2: отмена операций с помощью `ROLLBACK`:
 
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
ROLLBACK;
```
<img width="651" height="309" alt="image" src="https://github.com/user-attachments/assets/be8f974c-6dfa-47db-b5e2-085732d91a82" />

 
## 3. Демонстрация принципов ACID
 
Атомарность (Atomicity): либо выполняются все записи и обновление склада, либо ничего:
 
```sql
BEGIN;
INSERT INTO orders (customer_id, total) VALUES (1, 250.00);
INSERT INTO order_items (order_id, product_id, quantity) VALUES (1, 101, 2);
UPDATE inventory SET stock = stock - 2 WHERE product_id = 101;
COMMIT;
```
<img width="709" height="315" alt="image" src="https://github.com/user-attachments/assets/466bb430-409b-4a16-a732-a7f63c2f1cb1" />

 
Согласованность (Consistency): соблюдение внешних ключей и ограничений:
 
```sql
BEGIN;
INSERT INTO customers (name, email) VALUES ('John Doe', 'john@email.com');
INSERT INTO orders (customer_id, total) VALUES (LASTVAL(), 100.00);
COMMIT;
```
<img width="725" height="300" alt="image" src="https://github.com/user-attachments/assets/2b4e9b2b-5038-468e-9573-d67d1bd366f5" />

 
## 4. Уровни изоляции транзакций (Isolation Levels)
 
`READ UNCOMMITTED` (низший уровень):
 
```sql
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT * FROM accounts;
COMMIT;
```
<img width="500" height="315" alt="image" src="https://github.com/user-attachments/assets/9272e26d-7d25-497a-9e8f-521d0bfad30d" />

 
`READ COMMITTED` (уровень по умолчанию в PostgreSQL):
 
```sql
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
SELECT * FROM accounts WHERE balance > 1000;
COMMIT;
```
<img width="497" height="314" alt="image" src="https://github.com/user-attachments/assets/24a82e9d-1b18-4c4c-b006-1f14aa39fa6f" />

 
`REPEATABLE READ` (защищает от неповторяющегося чтения):
 
```sql
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT * FROM accounts WHERE balance > 1000;
COMMIT;
```
<img width="505" height="307" alt="image" src="https://github.com/user-attachments/assets/27835c71-4e1c-499d-b93b-46343173d0b1" />

 
`SERIALIZABLE` (полная изоляция процессов):
 
```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT * FROM accounts;
UPDATE accounts SET balance = balance * 1.05;
COMMIT;
```
<img width="475" height="307" alt="image" src="https://github.com/user-attachments/assets/555b5500-a699-4eeb-bacf-12b7518a2388" />

 
Установка уровня изоляции для всей сессии:
 
```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```
<img width="612" height="311" alt="image" src="https://github.com/user-attachments/assets/01ee5586-ec52-44bb-a993-c3cb57d25add" />

 
## 5. Использование точек сохранения (Savepoints)
 
Базовый откат к `SAVEPOINT`:
 
```sql
BEGIN;
INSERT INTO customers (name, email) VALUES ('Alice', 'alice@email.com');
SAVEPOINT after_customer_insert;
 
INSERT INTO orders (customer_id, total) VALUES (1, 500.00);
ROLLBACK TO SAVEPOINT after_customer_insert;
 
INSERT INTO orders (customer_id, total) VALUES (1, 300.00);
COMMIT;
```
<img width="692" height="313" alt="image" src="https://github.com/user-attachments/assets/97a6c767-39cb-4762-9660-622cd29c6a51" />

 
Множественные точки сохранения и освобождение (`RELEASE`):
 
```sql
BEGIN;
INSERT INTO products (name, price) VALUES ('Laptop', 999.99);
SAVEPOINT sp1;
 
INSERT INTO products (name, price) VALUES ('Mouse', 25.99);
SAVEPOINT sp2;
 
INSERT INTO products (name, price) VALUES ('Invalid Product', -50.00);
ROLLBACK TO SAVEPOINT sp2;
 
INSERT INTO products (name, price) VALUES ('Keyboard', 79.99);
RELEASE SAVEPOINT sp1;
COMMIT;
```
<img width="679" height="319" alt="image" src="https://github.com/user-attachments/assets/8103f5b4-eac9-4c4d-b7e6-2c0a1e75a712" />

 
## 6. Обработка ошибок в PL/pgSQL
 
```sql
DO $$
DECLARE
    insufficient_funds EXCEPTION;
    current_balance DECIMAL;
BEGIN
    SELECT balance INTO current_balance FROM accounts WHERE account_id = 1;
 
    IF current_balance < 100 THEN
        RAISE insufficient_funds;
    END IF;
 
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
 
EXCEPTION
    WHEN insufficient_funds THEN
        RAISE NOTICE 'Transaction failed: Insufficient funds';
        ROLLBACK;
END $$;
```
<img width="652" height="378" alt="image" src="https://github.com/user-attachments/assets/2d194362-89e6-4563-9c80-f2877ab28b69" />

 
## 7. Явный контроль и мониторинг блокировок (Locks)
 
Явный контроль транзакции для нескольких связанных операций:
 
```sql
BEGIN;
INSERT INTO audit_log (action, timestamp) VALUES ('user_creation', NOW());
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
INSERT INTO user_preferences (user_id, theme) VALUES (currval('users_user_id_seq'), 'dark');
COMMIT;
```
<img width="794" height="322" alt="image" src="https://github.com/user-attachments/assets/60bf29eb-fc22-41d4-b970-4a2f60daf66f" />

 
Запрос для обнаружения заблокированных транзакций и дедлоков:
 
```sql
SELECT
    blocked_locks.pid AS blocked_pid,
    blocked_activity.usename AS blocked_user,
    blocking_locks.pid AS blocking_pid,
    blocking_activity.usename AS blocking_user,
    blocked_activity.query AS blocked_statement
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_stat_activity blocked_activity
    ON blocked_activity.pid = blocked_locks.pid
JOIN pg_catalog.pg_locks blocking_locks
    ON blocking_locks.locktype = blocked_locks.locktype
WHERE NOT blocked_locks.granted;
```
<img width="760" height="293" alt="image" src="https://github.com/user-attachments/assets/1d012836-006d-4aa9-a27a-d932f3d7f4fc" />
