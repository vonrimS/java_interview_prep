# Работа с `HQL`

[назад](../README.md)


* [HQL и в чем его цель](#hql-и-в-чем-его-цель)
* [Основная разница между HQL и SQL](#основная-разница-между-hql-и-sql)
* [Как создать запрос HQL в Hibernate](#как-создать-запрос-hql-в-hibernate)
* [Какие основные конструкции языка поддерживаются в HQL](#какие-основные-конструкции-языка-поддерживаются-в-hql)
* [Как выполнить выборку данных из БД с использованием HQL](#как-выполнить-выборку-данных-из-бд-с-использованием-hql)
* [Как передавать параметры в HQL-запросы](#как-передавать-параметры-в-hql-запросы)
* [Какие операторы сравнения и логические операторы поддерживает HQL](#какие-операторы-сравнения-и-логические-операторы-поддерживает-hql)
* [Как сортировать результаты запроса HQL](#как-сортировать-результаты-запроса-hql)

Как выполнить соединение (JOIN) таблиц в HQL?

Какие агрегатные функции (например, SUM, AVG, COUNT) поддерживаются в HQL?

Как использовать псевдонимы (aliases) для таблиц и полей в HQL?

Как обрабатывать NULL-значения в HQL?

Какие функции поддерживает HQL для манипуляции данными (например, CONCAT, SUBSTRING)?

Как создать подзапрос (subquery) в HQL?

Как использовать выражения CASE и WHEN в HQL?

Какие методы доступа к данным (например, getter и setter) используются при работе с HQL?

Какие типы данных могут быть использованы в HQL?

Как обработать результаты запроса HQL и получить объекты Java?

Как обработать исключения, возникающие при выполнении запросов HQL?

Как настроить кэширование запросов в Hibernate с использованием HQL?


## `HQL` и в чем его цель

`HQL` (**Hibernate Query Language**) - это объектно-ориентированный язык запросов, предназначенный для выполнения запросов к базе данных в `Hibernate`, который является одной из популярных реализаций `Java Persistence API (JPA)`. Главной целью `HQL` является предоставление удобного и объектно-ориентированного способа доступа к данным в базе данных, без необходимости написания `SQL`-запросов напрямую.

### Основные цели и характеристики `HQL` включают:

1. `Объектно-ориентированный подход`: `HQL` позволяет выражать запросы к базе данных в терминах объектов и их свойств, что делает код более читаемым и обеспечивает легкость работы с объектами в рамках объектно-ориентированного программирования.

2. `Абстракция от базы данных`: `HQL` абстрагирует приложение от конкретной базы данных, позволяя разработчикам писать запросы, не зависящие от используемой `СУБД`.

3. `Поддержка наследования и ассоциаций`: `HQL` позволяет работать с объектами и их ассоциациями, включая наследование, многие-ко-многим и другие отношения между объектами.

4. `Параметризованные запросы`: `HQL` поддерживает передачу параметров в запросы, что делает их более гибкими и безопасными с точки зрения `SQL`-инъекций.

5. `Кеширование запросов`: `Hibernate` может кэшировать результаты запросов `HQL`, что повышает производительность приложения.

6. `Поддержка различных СУБД`: `Hibernate` может работать с различными базами данных, включая `MySQL`, `PostgreSQL`, `Oracle`, и другие, посредством соответствующих диалектов.

7. `Уровень абстракции для операций CRUD`: `HQL` позволяет выполнять операции создания, чтения, обновления и удаления данных (`CRUD`) с использованием объектных запросов.

8. `Интеграция с объектами Java`: Результаты запросов `HQL` могут быть легко преобразованы в объекты `Java`, что упрощает работу с данными в приложении.

***
Итак, `HQL` предоставляет удобный способ выполнения запросов к базе данных, используя объектно-ориентированный подход, и позволяет разработчикам более эффективно взаимодействовать с данными в приложениях, работающих с `Hibernate`.

[наверх](#работа-с-hql)

## Основная разница между `HQL` и `SQL`

Основная разница между `HQL` (**Hibernate Query Language**) и `SQL` (**Structured Query Language**) заключается в следующем:

### 1. Объектно-ориентированный подход vs. Реляционный подход:

* `HQL`: HQL является объектно-ориентированным языком запросов. Он оперирует объектами и их свойствами, а не таблицами и столбцами. Запросы `HQL` строятся в терминах классов и их полей.

* `SQL`: SQL, с другой стороны, ориентирован на реляционные базы данных и работает с таблицами, столбцами и отношениями между ними.

### 2. Способ обращения к данным:

* `HQL`: HQL позволяет обращаться к данным с использованием объектно-ориентированной нотации, используя сущности и их свойства. Например, вы можете выполнить запрос к объекту `User`, а не к таблице `users`.

* `SQL`: SQL требует явного указания таблиц и столбцов, что делает его менее абстрактным по сравнению с `HQL`.

### 3. Уровень абстракции:

* `HQL`: HQL предоставляет более высокий уровень абстракции, что делает его более гибким и позволяет разработчикам работать с объектами Java непосредственно.

* `SQL`: SQL ориентирован на работу с данными на уровне базы данных и требует знания структуры таблиц и схемы базы данных.

### 4. Поддержка баз данных:

* `HQL`: HQL, как правило, используется с Hibernate, который предоставляет абстракцию над различными СУБД. Это означает, что `HQL` запросы могут быть переносимыми между разными СУБД без изменения синтаксиса запросов.

* `SQL`: SQL запросы зависят от конкретной СУБД, и синтаксис SQL может различаться между разными системами. Необходимо писать `SQL` запросы, учитывая специфику используемой СУБД.


### 5. Преимущества и недостатки:

* `HQL`: HQL упрощает работу с объектами и более нативно интегрирован в объектно-ориентированные приложения. Он может предоставить более высокий уровень абстракции и уменьшить зависимость от конкретной СУБД. Однако, он может быть менее эффективным в случаях, когда требуются сложные операции или неточно подходит к реляционным данным.

* `SQL`: SQL предоставляет полный контроль над запросами и более точную спецификацию того, что делается в базе данных. Это может быть более подходящим в случаях, когда требуются сложные и оптимизированные запросы.

***
В зависимости от требований приложения и структуры данных, один из этих языков запросов может быть более подходящим выбором. В некоторых случаях также возможно совмещение `HQL` и `SQL` в одном запросе для достижения необходимой гибкости и эффективности.


[наверх](#работа-с-hql)

## Как создать запрос `HQL` в `Hibernate`

Для создания запроса `HQL` (**Hibernate Query Language**) в Hibernate, вы можете использовать объект `org.hibernate.query.Query`. `HQL` позволяет выполнять запросы к базе данных с использованием объектно-ориентированного синтаксиса. Вот как создать и выполнить запрос `HQL` в `Hibernate`:

### 1. Получите текущую сессию `Hibernate`:
Прежде чем создавать запрос `HQL`, убедитесь, что у вас есть текущая сессия `Hibernate`. Сессия представляет собой контекст взаимодействия с базой данных. Вы можете получить сессию следующим образом:

```java
Session session = sessionFactory.getCurrentSession();
```
Где `sessionFactory` - это экземпляр `SessionFactory`, созданный в вашем приложении.

### 2. Создайте запрос `HQL`:
Создайте объект `Query`, используя `HQL`-запрос. Например:

```java
String hql = "FROM User WHERE username = :username";
Query<User> query = session.createQuery(hql, User.class);
```
В этом примере, `User` - это имя сущности (`Entity`), с которой вы хотите выполнить запрос. `:username` - это параметр запроса.

### 3. Установите параметры запроса (по необходимости):
Если ваш запрос содержит параметры, вы можете установить их значения с помощью метода `setParameter`:

```java
query.setParameter("username", "johndoe");
```
### 4. Выполните запрос и получите результат:
Выполните запрос с помощью метода `list()`, `uniqueResult()`, `getResultList()`, или другого подходящего метода, в зависимости от того, ожидается ли одиночное значение или список результатов:

```java
List<User> users = query.getResultList();
```
В этом примере, результат будет представлен списком объектов `User`, удовлетворяющих условиям запроса.

### 5. Обработайте результаты запроса:
Обработайте результаты запроса в соответствии с вашими потребностями. Вы можете перебирать объекты, полученные в результате запроса, и выполнять необходимые операции.

### 6. Завершите транзакцию (при необходимости):
Если вы выполняете запрос в рамках транзакции, не забудьте завершить транзакцию после завершения операций с данными:

```java
session.getTransaction().commit();
```
Либо, в случае ошибки, откатите транзакцию:

```java
session.getTransaction().rollback();
```
Таким образом, вы можете создать и выполнить запрос `HQL` в `Hibernate`, чтобы извлечь данные из базы данных с использованием объектно-ориентированного синтаксиса.

[наверх](#работа-с-hql)

## Какие основные конструкции языка поддерживаются в `HQL`

`HQL` (Hibernate Query Language) поддерживает множество конструкций и операторов для выполнения запросов к базе данных. 

Вот основные конструкции и операторы, поддерживаемые в `HQL`:

### 1. `SELECT`
Используется для выборки данных из базы данных. Вы можете выбирать все поля сущности или определенные поля.

```
SELECT e FROM Employee e
```
```
SELECT e.firstName, e.lastName FROM Employee e
```

### 2. `FROM`
Указывает, из какой сущности (`Entity`) следует извлекать данные.

```
FROM User
```

### 3. `WHERE`
Определяет условие, которому должны удовлетворять данные, чтобы быть включенными в результат запроса.

```
FROM Product p WHERE p.price > 100
```

### 4. `ORDER` BY
Используется для сортировки результатов запроса.

```
FROM Employee e ORDER BY e.lastName ASC
```

### 5. `GROUP` BY
Группирует данные по определенному полю или полям.

```
SELECT COUNT(*) FROM Order o GROUP BY o.customer
```

### 6. `HAVING`
Позволяет фильтровать результаты группировки.

```
SELECT o.customer, COUNT(*) FROM Order o GROUP BY o.customer HAVING COUNT(*) > 5
```

### 7. `JOIN`
Используется для соединения нескольких сущностей на основе ассоциаций между ними.

```
FROM Employee e JOIN e.department d
```
```
FROM Customer c LEFT JOIN c.orders o
```

### 8. `INNER` JOIN
Объединяет данные из двух сущностей, возвращая только те строки, для которых есть соответствующие значения в обеих сущностях.
```
SELECT b, a
FROM Book b
INNER JOIN b.author a
```
В этом запросе `INNER JOIN` объединяет сущность `Book` с сущностью `Author` на основе связи `authorId`. Результат будет содержать только книги, у которых есть соответствующие авторы.

### 9. `LEFT` JOIN
Также объединяет данные из двух сущностей, но возвращает все строки из левой (первой) сущности и соответствующие строки из правой (второй) сущности. Если нет соответствующих значений в правой сущности, поля из правой сущности будут равны `null`.
```
SELECT c, o
FROM Customer c
LEFT JOIN c.orders o
```
Этот запрос `LEFT JOIN` вернет всех клиентов, включая тех, у которых нет заказов. Для клиентов без заказов, соответствующие поля заказов будут `null`.

### 10. `RIGHT` JOIN
Возвращает все строки из правой (второй) сущности и соответствующие строки из левой (первой) сущности. Если нет соответствующих значений в левой сущности, поля из левой сущности будут равны `null`.

```
SELECT d, e
FROM Department d
RIGHT JOIN d.employees e
```

### 11. `DISTINCT`
Используется для выбора уникальных результатов.

```
SELECT DISTINCT p.category FROM Product p
```

### 12. `INSERT` INTO
Выполняет вставку данных в базу данных.

```
INSERT INTO Employee (firstName, lastName) VALUES ('John', 'Doe')
```

### 13. `UPDATE`
Обновляет данные в базе данных.

```
UPDATE Product p SET p.price = 200 WHERE p.category = 'Electronics'
```

### 14. `DELETE`
Удаляет данные из базы данных.

```
DELETE FROM Product p WHERE p.price < 50
```
### 15. Параметры
Позволяют создавать параметризованные запросы для избегания SQL-инъекций.

```
FROM User u WHERE u.username = :username
```

### 16. Функции
`HQL` поддерживает множество встроенных функций, таких как `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `LOWER`, `UPPER`, `CONCAT`, и другие.

[наверх](#работа-с-hql)

## Как выполнить выборку данных из БД с использованием `HQL`

Для выполнения выборки данных из базы данных с использованием `HQL` (Hibernate Query Language) в `Hibernate`, вам потребуется создать запрос `HQL`, выполнить его и обработать результаты. Вот шаги, как это сделать:

### 1. Создайте запрос `HQL`:
Начните с создания запроса `HQL`, в котором указываете, какие данные вы хотите выбрать. Запрос `HQL` строится в терминах сущностей и их свойств.

```java
String hql = "SELECT p FROM Product p WHERE p.price > :minPrice";
```
### 2. Получите текущую сессию `Hibernate`:
Вам потребуется доступ к текущей сессии `Hibernate` для выполнения запроса.

```java
Session session = sessionFactory.getCurrentSession();
```

### 3. Создайте объект запроса:
Используйте созданный запрос `HQL` для создания объекта запроса с помощью метода `createQuery`. Укажите тип объектов, которые ожидаете получить.

```java
Query<Product> query = session.createQuery(hql, Product.class);
```

### 4. Установите параметры запроса (по необходимости):
Если ваш запрос содержит параметры, установите их значения с помощью метода `setParameter`.

```java
query.setParameter("minPrice", 100.0);
```

### 5. Выполните запрос и получите результаты:
Выполните запрос, используя методы, соответствующие вашим ожиданиям, такие как `list()`, `uniqueResult()`, `getResultList()` или другие.

```java
List<Product> expensiveProducts = query.getResultList();
```

### 6. Обработайте результаты запроса:
Обработайте результаты запроса в соответствии с вашими потребностями. В данном случае, вы получили список объектов `Product`, удовлетворяющих условиям запроса.

```java
for (Product product : expensiveProducts) {
    System.out.println("Product: " + product.getName() + ", Price: " + product.getPrice());
}
```

### 7. Завершите транзакцию (при необходимости):
Если вы выполняете запрос в рамках транзакции, не забудьте завершить транзакцию после завершения операций с данными.

```java
session.getTransaction().commit();
```
Таким образом, вы можете выполнить выборку данных из базы данных с использованием `HQL` в `Hibernate`, используя объектно-ориентированный синтаксис и обработать результаты выборки в своем приложении.

[наверх](#работа-с-hql)

## Как передавать параметры в `HQL`-запросы

Для передачи параметров в `HQL` (Hibernate Query Language) запросы в `Hibernate`, вы можете использовать именованные параметры. Именованные параметры в `HQL` начинаются с двоеточия (`:`) и могут быть использованы в запросе. Вот как передавать параметры в `HQL`-запросы:

### 1. Определите параметр в `HQL`-запросе:
В вашем `HQL`-запросе укажите имя параметра, начиная с двоеточия. Например:

```java
String hql = "SELECT p FROM Product p WHERE p.price > :minPrice";
```
В этом запросе `:minPrice` - это именованный параметр.

### 2. Создайте объект запроса и установите параметры:
Создайте объект запроса `HQL`, используя метод `createQuery`. Затем установите значение параметра с помощью метода `setParameter`, указав имя параметра и его значение.

```java
Query<Product> query = session.createQuery(hql, Product.class);
query.setParameter("minPrice", 100.0);
```
В этом примере, `"minPrice"` - это имя параметра, которое соответствует именованному параметру `:minPrice` в `HQL`-запросе.

### 3. Выполните запрос и получите результаты:
После установки параметров выполните запрос и получите результаты, как обычно.

```java
List<Product> expensiveProducts = query.getResultList();
```
Запрос будет выполнен с учетом переданного значения параметра `"minPrice"`.

Преимущество использования именованных параметров в `HQL` состоит в том, что это делает ваш код более безопасным относительно `SQL`-инъекций, и позволяет переиспользовать запросы с разными значениями параметров.

[наверх](#работа-с-hql)

## Какие операторы сравнения и логические операторы поддерживает `HQL`

`HQL` (Hibernate Query Language) поддерживает широкий набор операторов сравнения и логических операторов для создания сложных запросов. Вот некоторые из наиболее распространенных операторов и операторов, которые поддерживаются в `HQL`:

### Операторы сравнения:

1. Проверка на равенство.
```
WHERE age = 25
```
2. Проверка на неравенство.
```
WHERE status != 'inactive'
```
```
WHERE status <> 'inactive'
```

3. Меньше чем.
```
WHERE price < 100
```

4. Больше чем.
```
WHERE salary > 50000
```

5. Меньше или равно.
```
WHERE quantity <= 10
```

6. Больше или равно.
```
WHERE rating >= 3.5
```

7. Значение находится в заданном диапазоне.
```
WHERE age BETWEEN 18 AND 35
```

8. Поиск с использованием шаблона (поддерживает символы подстановки % и _).
```
WHERE name LIKE 'J%'
```

9. Проверка на NULL.
```
WHERE email IS NULL
```

10. Проверка на неравенство NULL.
```
WHERE phone IS NOT NULL
```

### Логические операторы:

1. Логическое И.
```
WHERE age > 18 AND salary > 50000
```

2. Логическое ИЛИ.
```
WHERE status = 'active' OR status = 'pending'
```

3. Логическое НЕ.
```
WHERE NOT (age < 18)
```

4. Значение находится в списке значений.
```
WHERE department IN ('HR', 'Finance', 'IT')
```
5. Значение не находится в списке значений.
```
WHERE role NOT IN ('Admin', 'Superuser')
```

Эти операторы сравнения и логические операторы позволяют вам создавать разнообразные условия в запросах `HQL` для выборки, фильтрации и сортировки данных в соответствии с вашими потребностями. Вы можете комбинировать их для создания сложных запросов.

[наверх](#работа-с-hql)

## Как сортировать результаты запроса `HQL`

Для сортировки результатов запроса `HQL` (Hibernate Query Language) вы можете использовать ключевое слово `ORDER BY`. С помощью `ORDER BY` вы указываете, по какому полю или полям следует провести сортировку, и в каком порядке (по возрастанию или убыванию). Вот как сортировать результаты запроса `HQL`:

### 1. `Сортировка по одному полю`:

Для сортировки по одному полю, используйте `ORDER BY` с именем поля.

```java
String hql = "SELECT p FROM Product p ORDER BY p.price ASC";
```
В этом примере, результаты будут отсортированы по полю price в порядке возрастания (`ASC`).

### 2. `Сортировка по нескольким полям`:
Для сортировки по нескольким полям, перечислите их через запятую.

```java
String hql = "SELECT e FROM Employee e ORDER BY e.lastName ASC, e.firstName ASC";
```
В этом примере, результаты сортируются сначала по полю `lastName` в порядке возрастания, а затем по полю `firstName` в порядке возрастания.

### 3. `Сортировка в обратном порядке`:
Для сортировки в обратном порядке, используйте ключевое слово `DESC`.

```java
String hql = "SELECT p FROM Product p ORDER BY p.price DESC";
```
В этом примере, результаты будут отсортированы по полю `price` в порядке убывания.

### 4. Сортировка по вычисляемым значениям или выражениям:
Вы можете использовать вычисляемые значения или выражения в `ORDER BY`. Например, сортировка по результату арифметического вычисления.

```java
String hql = "SELECT p FROM Product p ORDER BY (p.price - p.discount) DESC";
```
В этом примере, результаты будут отсортированы по разнице между `price` и `discount` в порядке убывания.

### 5. Сортировка по псевдонимам или позиции:
Если вы используете псевдонимы в вашем запросе, вы можете сортировать по ним.

```java
String hql = "SELECT p AS product FROM Product p ORDER BY product.price ASC";
В этом примере, результаты сортируются по псевдониму product.

Лимитирование результатов:
Вы также можете добавить лимитирование (ограничение) к вашему запросу с помощью LIMIT (если ваша база данных поддерживает этот оператор) или методов Hibernate, таких как setMaxResults(), чтобы получить ограниченное количество записей.

Пример с использованием setMaxResults():

java
Copy code
query.setMaxResults(10);
В этом примере, будет получено только 10 записей из результата запроса.

Выполните запрос и обработайте результаты:
После добавления сортировки к вашему запросу, выполните его и обработайте результаты, как описано в предыдущем ответе.

Сортировка в HQL позволяет вам управлять порядком вывода результатов запроса и выбирать данные в нужной последовательности.






[наверх](#работа-с-hql)

##

[наверх](#работа-с-hql)

##

[наверх](#работа-с-hql)

##

[наверх](#работа-с-hql)

##

[наверх](#работа-с-hql)

##

[наверх](#работа-с-hql) | 
[назад](../README.md)