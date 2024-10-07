# Топ-20 SQL задач

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

## 3. Топ-25 задач по `SQL`

1. Получить уникальные должности сотрудников
<details>
 <summary>Решение 1</summary> 
 </br>

    SELECT DISTINCT position FROM employees;

</details>
<br>

2. Подсчитать количество сотрудников в каждой должности
<details>
<summary>Решение 2</summary> 
</br>

    SELECT position, COUNT(*) AS num_employees
    FROM employees
    GROUP BY position;

</details>
<br>

3. Найти сотрудников, чья зарплата больше $100,000
<details>
<summary>Решение 3</summary> 
</br>

    SELECT name, salary
    FROM employees
    WHERE salary > 100000;
</details>
<br>

4. Соединить таблицы сотрудников и менеджеров
<details>
<summary>Решение 4</summary> 
</br>

    SELECT e.name AS employee_name, m.name AS manager_name
    FROM employees e
    LEFT JOIN employees m ON e.manager_id = m.id;

</details>
<br>

5. Посчитать общее количество продаж для каждого продукта
<details>
<summary>Решение 5</summary> 
</br>

    SELECT p.product_name, SUM(s.quantity) AS total_sales
    FROM products p
    JOIN sales s ON p.id = s.product_id
    GROUP BY p.product_name;

</details>
<br>


6. Получить продукты с более чем 5 продажами 
<details>
<summary>Решение 6</summary> 
</br>
    SELECT product_name, SUM(quantity) AS total_quantity
    FROM products p
    JOIN sales s ON p.id = s.product_id
    GROUP BY product_name
    HAVING SUM(quantity) > 5;

</details>
<br>

7. Найти всех менеджеров, у которых больше 2 подчиненных
<details>
<summary>Решение 7</summary> 
</br>

    SELECT m.name AS manager_name, COUNT(e.id) AS num_subordinates
    FROM employees e
    JOIN employees m ON e.manager_id = m.id
    GROUP BY m.name
    HAVING COUNT(e.id) > 2;

</details>
<br>

8. Найти топ-3 самых дорогих продуктов
<details>
<summary>Решение 8</summary> 
</br>

    SELECT product_name, price
    FROM products
    ORDER BY price DESC
    LIMIT 3;

</details>
<br>

9. Добавить номера строк каждому сотруднику в пределах позиции
<details>
<summary>Решение 9</summary> 
</br>

    SELECT name, position, 
        ROW_NUMBER() OVER (PARTITION BY position ORDER BY salary DESC) AS row_num
    FROM employees;

</details>
<br>

10. Присвоить метку зарплатам сотрудников
<details>
<summary>Решение 10</summary> 
</br>

    SELECT name, salary,
        CASE 
            WHEN salary > 100000 THEN 'High Salary'
            WHEN salary BETWEEN 60000 AND 100000 THEN 'Average Salary'
            ELSE 'Low Salary'
        END AS salary_category
    FROM employees;

</details>
<br>

11. Обновить зарплату всех инженеров на 5% больше
<details>
<summary>Решение 11</summary> 
</br>
    UPDATE employees
    SET salary = salary * 1.05
    WHERE position = 'Engineer';

</details>
<br>

12. Удалить всех сотрудников с зарплатой ниже $60,000 
<details>
<summary>Решение 12</summary> 
</br>

    DELETE FROM employees
    WHERE salary < 60000;

</details>
<br>

13. Найти сотрудников с одинаковыми зарплатами
<details>
<summary>Решение 13</summary> 
</br>

    SELECT salary, COUNT(*) AS num_employees
    FROM employees
    GROUP BY salary
    HAVING COUNT(*) > 1;

</details>
<br>

14. Найти сотрудников, которые были наняты в этом году
<details>
<summary>Решение 14</summary> 
</br>

    SELECT name, hire_date
    FROM employees
    WHERE YEAR(hire_date) = YEAR(CURDATE());

</details>
<br>

15. Объединить информацию о всех сотрудниках и продуктах (UNION)
<details>
<summary>Решение 15</summary> 
</br>

    SELECT name AS entity_name, 'Employee' AS entity_type FROM employees
    UNION
    SELECT product_name AS entity_name, 'Product' AS entity_type FROM products;

</details>
<br>

16. Создать сводную таблицу для количества продаж по категории продуктов
<details>
<summary>Решение 16</summary> 
</br>

    SELECT category,
        SUM(CASE WHEN s.sale_date BETWEEN '2023-01-01' AND '2023-06-30' THEN s.quantity ELSE 0 END) AS sales_first_half,
        SUM(CASE WHEN s.sale_date BETWEEN '2023-07-01' AND '2023-12-31' THEN s.quantity ELSE 0 END) AS sales_second_half
    FROM products p
    LEFT JOIN sales s ON p.id = s.product_id
    GROUP BY category;

</details>
<br>

17. Построить иерархию сотрудников и их менеджеров 
<details>
<summary>Решение 17</summary> 
</br>

    WITH RECURSIVE hierarchy AS (
        SELECT id, name, manager_id
        FROM employees
        WHERE manager_id IS NULL
        UNION ALL
        SELECT e.id, e.name, e.manager_id
        FROM employees e
        JOIN hierarchy h ON e.manager_id = h.id
    )
    SELECT * FROM hierarchy;

</details>
<br>

18. Использовать COALESCE для замены NULL менеджеров на 'No Manager'

<details>
<summary>Решение 18</summary> 
</br>

    SELECT name, COALESCE((SELECT name FROM employees WHERE id = manager_id), 'No Manager') AS manager_name
    FROM employees;

</details>
<br>

19. Получить среднюю зарплату по каждой должности
<details>
<summary>Решение 19</summary> 
</br>

    SELECT position, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY position;

</details>
<br>

20. Найти сотрудников, которые отсутствовали более 2 дней подряд в логах (Gaps and Islands)
<details>
<summary>Решение 20</summary> 
</br>

    SELECT user_id, MIN(login_date) AS start_date, MAX(login_date) AS end_date
    FROM (
        SELECT user_id, login_date,
            ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) -
            DENSE_RANK() OVER (ORDER BY login_date) AS group_id
        FROM logins
    ) t
    GROUP BY user_id, group_id
    HAVING DATEDIFF(MAX(login_date), MIN(login_date)) > 2;

</details>
<br>

21. Подсчитать общую зарплату для каждого менеджера и его подчиненных (WITH RECURSIVE, SUM)
<details>
<summary>Решение 21</summary> 
</br>

    WITH RECURSIVE salary_hierarchy AS (
        SELECT id, name, salary, manager_id
        FROM employees
        WHERE manager_id IS NULL
        UNION ALL
        SELECT e.id, e.name, e.salary, e.manager_id
        FROM employees e
        JOIN salary_hierarchy h ON e.manager_id = h.id
    )
    SELECT manager_id, SUM(salary) AS total_salary
    FROM salary_hierarchy
    GROUP BY manager_id;

</details>
<br>

22. Найти количество продаж в каждой категории продуктов, отсортированных по убыванию (JOIN, ORDER BY)

<details>
<summary>Решение 22</summary> 
</br>

    SELECT category, COUNT(s.id) AS sales_count
    FROM products p
    JOIN sales s ON p.id = s.product_id
    GROUP BY category
    ORDER BY sales_count DESC;

</details>
<br>

23. Использовать оконные функции для расчета скользящего среднего по продажам (AVG, ROWS BETWEEN)

<details>
<summary>Решение 23</summary> 
</br>

    SELECT sale_date, quantity,
        AVG(quantity) OVER (ORDER BY sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
    FROM sales;

</details>
<br>

24. Использовать EXPLAIN для анализа производительности запроса к таблице sales

<details>
<summary>Решение 24</summary> 
</br>

    EXPLAIN SELECT * FROM sales WHERE sale_date BETWEEN '2023-01-01' AND '2023-12-31';

</details>
<br>

25. Разделить таблицу sales на партиции по году продаж

<details>
<summary>Решение 25</summary> 
</br>

    CREATE TABLE sales_2022 PARTITION OF sales
    FOR VALUES FROM ('2022-01-01') TO ('2022-12-31');

    CREATE TABLE sales_2023 PARTITION OF sales
    FOR VALUES FROM ('2023-01-01') TO ('2023-12-31');

</details>
<br>
