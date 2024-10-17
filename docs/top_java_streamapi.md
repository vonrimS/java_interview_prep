# Топ-20 `Java` задач по `Stream API`


20. Обработка исключений в Stream API (Handling Exceptions in Streams)

Напишите пример, где в Stream API обрабатываются исключения внутри лямбда-выражений.

<details>
 <summary>Решение 20</summary> 
 </br>

    // ---try-catch---

    public static void main(String[] args) {
        List<String> numbers = Arrays.asList("1", "2", "a", "3");

        // Пример обработки исключений внутри лямбда-выражений
        List<Integer> result = numbers.stream()
            .map(number -> {
                try {
                    return Integer.parseInt(number);  // Попытка преобразования строки в число
                } catch (NumberFormatException e) {
                    System.out.println("Invalid number format for: " + number);
                    return null;  // Возвращаем null, если возникло исключение
                }
            })
            .filter(num -> num != null)  // Фильтруем значения null
            .toList();

        System.out.println("Parsed numbers: " + result);
    }

    // ---with-wrapper-function---

    public static void main(String[] args) {
        List<String> numbers = Arrays.asList("1", "2", "a", "3");

        // Используем обертку для обработки исключений
        List<Integer> result = numbers.stream()
            .map(wrapper(Integer::parseInt))  // Обрабатываем исключения через обертку
            .filter(num -> num != null)  // Фильтруем null значения
            .toList();

        System.out.println("Parsed numbers: " + result);
    }

    // Вспомогательный метод для обработки исключений
    public static <T, R> Function<T, R> wrapper(Function<T, R> function) {
        return input -> {
            try {
                return function.apply(input);
            } catch (Exception e) {
                System.out.println("Exception occurred: " + e.getMessage() + " for input: " + input);
                return null;  // Возвращаем null в случае ошибки
            }
        };
    }

    // ---with-throw---

    public static void main(String[] args) {
        List<String> files = Arrays.asList("file1.txt", "file2.txt", "invalidFile");

        // Пример работы с проверяемыми исключениями (checked exceptions)
        List<String> result = files.stream()
            .map(file -> {
                try {
                    return readFile(file);  // Пробуем прочитать файл
                } catch (IOException e) {
                    throw new RuntimeException("Error reading file: " + file, e);  // Пробрасываем через RuntimeException
                }
            })
            .toList();

        System.out.println("Processed files: " + result);
    }

    // Метод, который может бросить проверяемое исключение (IOException)
    public static String readFile(String file) throws IOException {
        if ("invalidFile".equals(file)) {
            throw new IOException("File not found: " + file);
        }
        return "Content of " + file;
    }
    
</details>
<br>

1. **Фильтрация списка (Filtering List)**

Используя **Stream API**, отфильтруйте список строк, оставив только строки, начинающиеся с определенного символа.

<details>
 <summary>Решение 1</summary> 
 </br>

    public static List<String> filterStringsByChar(List<String> input, char firstChar){
        return input.stream()
                .filter(str -> !str.isEmpty() && str.charAt(0) == firstChar)
                .collect(Collectors.toList());
    }

</details>
<br>

2. **Группировка объектов (Grouping Objects)**

Используя `Stream API`, сгруппируйте объекты по их определенному полю и подсчитайте их количество в каждой группе.

<details>
 <summary>Решение 2</summary> 
 </br>
    
    import lombok.AllArgsConstructor;
    import lombok.Data;

    @Data
    @AllArgsConstructor
    public class Person {
        private String name;
        private String city;
    }

    // -------------


    public class GroupingByExample {
        public static void main(String[] args) {
            List<Person> people = Arrays.asList(
                new Person("John", "New York"),
                new Person("Anna", "Los Angeles"),
                new Person("Mike", "New York"),
                new Person("Sophia", "Los Angeles"),
                new Person("Paul", "Chicago")
            );

            Map<String, Long> peopleByCity = people.stream()
                    .collect(Collectors.groupingBy(Person::getCity, Collectors.counting()));

            peopleByCity.forEach((city, count) ->
                    System.out.println("City: " + city + ", Number of people: " + count));
        }
    }
    
</details>
<br>

3. **Поиск максимального значения в списке объектов (Max Value from List of Objects)**

Найдите объект с максимальным значением поля в списке, используя `Stream API`.

<details>
 <summary>Решение 3</summary> 
 </br>

    @Data
    @AllArgsConstructor
    @ToString
    public class Person {
        private String name;
        private int age;
    }

    // -------------

    public class MaxValueExample {
        public static void main(String[] args) {
            List<Person> people = Arrays.asList(
                    new Person("John", 25),
                    new Person("Anna", 30),
                    new Person("Mike", 35),
                    new Person("Sophia", 28),
                    new Person("Paul", 45)
            );

            Optional<Person> oldestPerson = people.stream()
                    .max(Comparator.comparingInt(Person::getAge));

            oldestPerson.ifPresent(person ->
                    System.out.println("Oldest person: " + person));
        }
    }
    
</details>
<br>

15. Преобразование списка в мапу (List to Map)

Напишите метод, который преобразует список объектов в Map, где ключами будут уникальные поля объектов.

<details>
 <summary>Решение 15</summary> 
 </br>

    @Data
    @AllArgsConstructor
    @ToString
    public class Person {
        private int id;
        private String name;
    }

    // --------

    public class ListToMapExample {

    public static Map<Integer, Person> convertListToMap(List<Person> people){
        // Преобразуем список в Map, где ключом является поле id, а значением — объект Person
        return people.stream()
                .collect(Collectors.toMap(
                        Person::getId,  // Ключом будет id
                        person -> person, // Значением будет сам объект
                        (existing, replacement) -> existing // В случае дублирования ключа оставляем первый объект
                ));
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
                new Person(1, "John"),
                new Person(2, "Anna"),
                new Person(3, "Mike"),
                new Person(2, "Sophia")  // Повторение id 2
        );

        // Преобразуем список в Map
        Map<Integer, Person> peopleMap = convertListToMap(people);

        // Выводим результат
        peopleMap.forEach((id, person) ->
                System.out.println("ID: " + id + " -> " + person));
    }

    
</details>
<br>

16. Объединение двух Map (Merging Maps)

Реализуйте метод, который объединяет два Map, разрешая конфликты путем суммирования значений.

<details>
 <summary>Решение 16</summary> 
 </br>

    public static Map<String, Integer> mergeMaps(Map<String, Integer> map1, Map<String, Integer> map2) {
        // Объединяем два Map, разрешая конфликты через суммирование значений
        return Stream.concat(map1.entrySet().stream(), map2.entrySet().stream())
                .collect(Collectors.toMap(
                        Map.Entry::getKey,   // Ключ
                        Map.Entry::getValue, // Значение
                        Integer::sum         // Разрешение конфликтов через суммирование
                ));
    }

    // -----

    public static void main(String[] args) {
        Map<String, Integer> map1 = new HashMap<>();
        map1.put("a", 1);
        map1.put("b", 2);
        map1.put("c", 3);

        Map<String, Integer> map2 = new HashMap<>();
        map2.put("b", 3);
        map2.put("c", 4);
        map2.put("d", 5);

        // Объединяем два Map
        Map<String, Integer> mergedMap = mergeMaps(map1, map2);

        // Выводим результат
        mergedMap.forEach((key, value) -> 
            System.out.println("Key: " + key + ", Value: " + value));
    }
    
</details>
<br>

17. Сортировка коллекции объектов (Sorting Collection)

Напишите метод для сортировки коллекции объектов по заданному полю в обратном порядке.

<details>
 <summary>Решение 17</summary> 
 </br>

    @Data
    @AllArgsConstructor
    @ToString
    public class Person {
        private String name;
        private int age;
    }

    // -----

    public static List<Person> sortByFieldInReverseOrder(List<Person> people) {
        // Сортируем по возрасту в обратном порядке
        return people.stream()
                .sorted(Comparator.comparingInt(Person::getAge).reversed())  // Сортировка по полю age в обратном порядке
                .collect(Collectors.toList());  // Собираем результат в список
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("John", 25),
            new Person("Anna", 30),
            new Person("Mike", 35),
            new Person("Sophia", 28)
        );

        // Сортируем коллекцию по возрасту в обратном порядке
        List<Person> sortedPeople = sortByFieldInReverseOrder(people);

        // Выводим результат
        sortedPeople.forEach(System.out::println);
    }
    
</details>
<br>

18. Удаление дубликатов из списка (Remove Duplicates)

Реализуйте метод, который удаляет дубликаты из List и возвращает список уникальных значений.

<details>
 <summary>Решение 18</summary> 
 </br>

    public static <T> List<T> removeDuplicates(List<T> list){
    return list.stream()
            .distinct()
            .toList();
    }

    // -----

    public static void main(String[] args) {

        var a = Arrays.asList("apple", "banana", "apple", "kiwi");

        List<String> uniqueList = TopJavaProblems.removeDuplicates(a);

        uniqueList.forEach(System.out::println);
    }
    
</details>
<br>


4. Фильтрация списка строк по длине:
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