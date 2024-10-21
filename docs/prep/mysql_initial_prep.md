# Подготовка контейнера и базы данных

[назад](../top_sql_problems.md)

## 1. Поднять базу данных `MySQL` в `Docker`
```bash
docker run --name mysql-problems-container \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=companyDB \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=user_password \
  -p 3300:3306 \
  -v mysql-data:/var/lib/mysql \
  -d mysql:8.0
```

## 2. Создать инфраструктуру для выполнения задач
1. Подключиться к базеданных в контейнере через `MySQL Dashboard`

2. Создать инфрастурктуру внутри для выполнения заданий
```sql
-- Использование базы данных
USE companyDB;

-- 1. Таблица employees (сотрудники и их менеджеры)
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    position VARCHAR(50),
    salary DECIMAL(10, 2),
    manager_id INT,
    hire_date DATE,
    FOREIGN KEY (manager_id) REFERENCES employees(id)
);

-- Наполнение таблицы employees данными
INSERT INTO employees (name, position, salary, manager_id, hire_date) VALUES 
('John Smith', 'CEO', 150000, NULL, '2015-01-10'),
('Sarah Johnson', 'CFO', 120000, 1, '2016-03-15'),
('Mike Lee', 'CTO', 125000, 1, '2016-05-20'),
('Anna Brown', 'Engineer', 90000, 3, '2018-07-12'),
('Sam Taylor', 'Engineer', 85000, 3, '2019-09-30'),
('James Davis', 'Finance Analyst', 75000, 2, '2017-02-14'),
('Emily White', 'Accountant', 60000, 2, '2018-11-25'),
('David Green', 'Engineer', 95000, 3, '2018-10-14');

-- 2. Таблица products (информация о продуктах)
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);

-- Наполнение таблицы products данными
INSERT INTO products (product_name, category, price) VALUES 
('Laptop', 'Electronics', 1000.00),
('Smartphone', 'Electronics', 800.00),
('Tablet', 'Electronics', 600.00),
('Headphones', 'Accessories', 150.00),
('Camera', 'Accessories', 500.00),
('Chair', 'Furniture', 200.00),
('Desk', 'Furniture', 350.00),
('Mouse', 'Accessories', 50.00),
('Keyboard', 'Accessories', 100.00),
('Monitor', 'Electronics', 300.00);

-- 3. Таблица sales (данные о продажах)
CREATE TABLE sales (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    sale_date DATE,
    quantity INT,
    amount DECIMAL(10, 2),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Наполнение таблицы sales данными
INSERT INTO sales (product_id, sale_date, quantity, amount) VALUES 
(1, '2023-01-10', 5, 5000.00),
(2, '2023-02-15', 10, 8000.00),
(3, '2023-02-20', 7, 4200.00),
(4, '2023-03-05', 15, 2250.00),
(5, '2023-04-01', 3, 1500.00),
(6, '2023-04-15', 8, 1600.00),
(7, '2023-05-01', 5, 1750.00),
(8, '2023-06-10', 20, 1000.00),
(9, '2023-07-15', 12, 1200.00),
(10, '2023-08-01', 6, 1800.00),
(1, '2023-09-10', 2, 2000.00),
(2, '2023-10-12', 3, 2400.00),
(3, '2023-11-05', 4, 2400.00),
(4, '2023-11-20', 6, 900.00);

-- Дополнительные данные для логов входа (логины сотрудников)
CREATE TABLE logins (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    login_date DATE,
    FOREIGN KEY (user_id) REFERENCES employees(id)
);

-- Наполнение таблицы logins данными
INSERT INTO logins (user_id, login_date) VALUES 
(4, '2023-01-01'), (4, '2023-01-02'), (4, '2023-01-03'),
(5, '2023-01-10'), (5, '2023-01-11'),
(6, '2023-01-15'), (6, '2023-01-16'), (6, '2023-01-17'),
(4, '2023-02-01'), (4, '2023-02-02'), (4, '2023-02-03'),
(5, '2023-02-10'), (5, '2023-02-11'),
(6, '2023-02-15'), (6, '2023-02-16'), (6, '2023-02-17');
```


[назад](../top_sql_problems.md)
