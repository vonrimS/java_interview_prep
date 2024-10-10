# Топ-20 `Java` задач по `Stream API`

Вот топ-20 задач для live coding собеседования по теме Java Stream API. Эти задачи помогут проверить навыки работы с потоками, фильтрацией, сортировкой, группировкой и другими операциями.

1. Фильтрация списка строк по длине:
Отфильтруйте список строк, оставив только те, длина которых больше 5 символов.

java
Copy code
List<String> filtered = list.stream()
    .filter(s -> s.length() > 5)
    .collect(Collectors.toList());
2. Подсчет уникальных элементов в списке:
Подсчитайте количество уникальных элементов в списке.

java
Copy code
long count = list.stream()
    .distinct()
    .count();
3. Поиск максимального элемента в списке чисел:
Найдите максимальное значение в списке целых чисел.

java
Copy code
Optional<Integer> max = list.stream()
    .max(Integer::compareTo);
4. Поиск минимального элемента в списке строк (по длине):
Найдите строку с минимальной длиной в списке строк.

java
Copy code
Optional<String> min = list.stream()
    .min(Comparator.comparingInt(String::length));
5. Преобразование списка строк в список их длин:
Преобразуйте список строк в список их длин.

java
Copy code
List<Integer> lengths = list.stream()
    .map(String::length)
    .collect(Collectors.toList());
6. Сортировка списка объектов по полю:
Отсортируйте список объектов Person по их возрасту.

java
Copy code
List<Person> sortedList = people.stream()
    .sorted(Comparator.comparingInt(Person::getAge))
    .collect(Collectors.toList());
7. Суммирование всех чисел в списке:
Посчитайте сумму всех элементов списка целых чисел.

java
Copy code
int sum = list.stream()
    .mapToInt(Integer::intValue)
    .sum();
8. Группировка объектов по полю:
Сгруппируйте объекты Person по городу.

java
Copy code
Map<String, List<Person>> groupedByCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity));
9. Подсчет количества элементов в каждой группе:
Подсчитайте количество объектов в каждой группе, сгруппированных по городу.

java
Copy code
Map<String, Long> countByCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity, Collectors.counting()));
10. Преобразование списка строк в строку через разделитель:
Преобразуйте список строк в одну строку, где строки разделены запятой.

java
Copy code
String result = list.stream()
    .collect(Collectors.joining(", "));
11. Фильтрация и преобразование:
Отфильтруйте список строк, оставив только те, которые начинаются с буквы "A", и преобразуйте их в верхний регистр.

java
Copy code
List<String> filtered = list.stream()
    .filter(s -> s.startsWith("A"))
    .map(String::toUpperCase)
    .collect(Collectors.toList());
12. Проверка, содержатся ли все элементы удовлетворяющие условию:
Проверьте, содержатся ли в списке только положительные числа.

java
Copy code
boolean allPositive = list.stream()
    .allMatch(n -> n > 0);
13. Поиск первого элемента, удовлетворяющего условию:
Найдите первый элемент в списке строк, который начинается с буквы "B".

java
Copy code
Optional<String> result = list.stream()
    .filter(s -> s.startsWith("B"))
    .findFirst();
14. Поиск любого элемента:
Найдите любой элемент в списке строк, который заканчивается на "ing".

java
Copy code
Optional<String> result = list.stream()
    .filter(s -> s.endsWith("ing"))
    .findAny();
15. Преобразование списка объектов в Map:
Преобразуйте список объектов Person в карту, где ключом будет имя, а значением — возраст.

java
Copy code
Map<String, Integer> map = people.stream()
    .collect(Collectors.toMap(Person::getName, Person::getAge));
16. Подсчет частоты слов в строке:
Подсчитайте, сколько раз каждое слово встречается в строке.

java
Copy code
Map<String, Long> wordCount = Arrays.stream(text.split("\\s+"))
    .collect(Collectors.groupingBy(word -> word, Collectors.counting()));
17. Проверка, есть ли хотя бы один элемент, удовлетворяющий условию:
Проверьте, есть ли в списке хотя бы одно число больше 100.

java
Copy code
boolean exists = list.stream()
    .anyMatch(n -> n > 100);
18. Преобразование потока в массив:
Преобразуйте поток строк в массив.

java
Copy code
String[] array = list.stream()
    .toArray(String[]::new);
19. Сгруппируйте числа на четные и нечетные:
Сгруппируйте список чисел на четные и нечетные.

java
Copy code
Map<Boolean, List<Integer>> partitioned = list.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
20. Пропустите первые N элементов и соберите остальные:
Пропустите первые 3 элемента списка и соберите оставшиеся.

java
Copy code
List<Integer> result = list.stream()
    .skip(3)
    .collect(Collectors.toList());