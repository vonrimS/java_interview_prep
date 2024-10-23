# Топ `SQL` задач


[назад](../README.md)

[подготовка базы данных](./prep/mysql_initial_prep.md)


1. Получить уникальные должности `position` сотрудников из таблицы `employees`
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

3. Найти сотрудников, чья зарплата больше `$100,000`
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

11. Обновить зарплату всех инженеров на `5%` больше
<details>
<summary>Решение 11</summary> 
</br>
    UPDATE employees
    SET salary = salary * 1.05
    WHERE position = 'Engineer';

</details>
<br>

12. Удалить всех сотрудников с зарплатой ниже `$60,000` 
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

15. Объединить информацию о всех сотрудниках и продуктах (`UNION`)
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

18. Использовать `COALESCE` для замены `NULL` менеджеров на `No Manager`

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

20. Найти сотрудников, которые отсутствовали более `2 дней` подряд в логах (`Gaps and Islands`)
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

21. Подсчитать общую зарплату для каждого менеджера и его подчиненных (`WITH RECURSIVE`, `SUM`)
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

22. Найти количество продаж в каждой категории продуктов, отсортированных по убыванию (`JOIN`, `ORDER BY`)

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

23. Использовать оконные функции для расчета скользящего среднего по продажам (`AVG`, `ROWS` `BETWEEN`)

<details>
<summary>Решение 23</summary> 
</br>

    SELECT sale_date, quantity,
        AVG(quantity) OVER (ORDER BY sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
    FROM sales;

</details>
<br>

24. Использовать `EXPLAIN` для анализа производительности запроса к таблице `sales`

<details>
<summary>Решение 24</summary> 
</br>

    EXPLAIN SELECT * FROM sales WHERE sale_date BETWEEN '2023-01-01' AND '2023-12-31';

</details>
<br>

25. Разделить таблицу `sales` на партиции по году продаж

<details>
<summary>Решение 25</summary> 
</br>

    CREATE TABLE sales_2022 PARTITION OF sales
    FOR VALUES FROM ('2022-01-01') TO ('2022-12-31');

    CREATE TABLE sales_2023 PARTITION OF sales
    FOR VALUES FROM ('2023-01-01') TO ('2023-12-31');

</details>
<br>
