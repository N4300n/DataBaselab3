# Работа с базами данных в PostgreSQL

### 1. Подключение через командную строку
```bash
psql -d postgres -U username
```

<img width="760" height="173" alt="image" src="https://github.com/user-attachments/assets/f355ce2e-685f-4bd1-bcf5-0c6a182e3337" />

### 2. Создание базы данных

```bash
CREATE DATABASE university;
```

<img width="657" height="189" alt="image" src="https://github.com/user-attachments/assets/10887c12-7395-4f15-a9fa-5a23225646ff" />
<img width="976" height="692" alt="image" src="https://github.com/user-attachments/assets/f4031fc6-de41-4a05-85c6-74703e3e7aaf" />

### 3. Удаление базы данных

```bash
DROP DATABASE university;
```

<img width="391" height="265" alt="image" src="https://github.com/user-attachments/assets/271f952a-279c-4876-a7a4-ac450eea4a49" />
<img width="344" height="90" alt="image" src="https://github.com/user-attachments/assets/5cea7816-8b68-4bbb-8ca1-0ed5177c038c" />

### 4. Переключение между базами данных

```bash
\c university
```

<img width="737" height="232" alt="image" src="https://github.com/user-attachments/assets/54975f27-574a-4c0a-aa57-25f7cc8c529e" />
