# Топ `java` задач

## 1. Алгоритмы и структуры данных

1. Реверс строки (String Reversal)

Напишите метод, который принимает строку и возвращает ее в обратном порядке без использования встроенных методов обратного прохода строки.
<details>
 <summary>Решение 1</summary> 
 </br>

    public static String reverseString(String input){
        StringBuilder reversed = new StringBuilder();
        char[] chars = input.toCharArray();

        for (int i = input.length()-1; i >= 0; i--){
            reversed.append(chars[i]);
        }

        return reversed.toString();
    }

</details>
<br>


2. Проверка палиндрома (Palindrome Check)

Напишите метод, который проверяет, является ли данная строка палиндромом (читается одинаково с обеих сторон).
<details>
 <summary>Решение 2</summary> 
 </br>

    public static boolean isPalindrome(String input){
        int len = input.length();

        for (int i = 0; i < len/2; i++){
            if (input.charAt(i) != input.charAt(len - i - 1)){
                return false;
            }
        }

        return true;
    }

</details>
<br>

3. Анаграммы (Anagram Check)

Напишите метод, который определяет, являются ли две строки анаграммами (имеют одинаковые символы в разном порядке).
<details>
 <summary>Решение 3</summary> 
 </br>

    public static boolean isAnagram(String a, String b){
        if (a.length() != b.length())
            return false;

        int[] count = new int[256]; // Массив для подсчета символов (ASCII 256 символов)

        // Увеличиваем счётчик для символов из строки 'a' и уменьшаем для символов из строки 'b'
        for (int i = 0; i < a.length(); i++) {
            count[a.charAt(i)]++;
            count[b.charAt(i)]--;
        }

        // Проверяем, что все значения в массиве равны нулю
        for (int i : count) {
            if (i != 0)
                return false;
        }

        return true;

    }
    
</details>
<br>

4. Найти наибольший элемент в массиве (Find Max Element)

Напишите функцию, которая находит наибольший элемент в массиве целых чисел.

<details>
 <summary>Решение 4</summary> 
 </br>

    public static int getBiggestElement(int[] input){
        int temp = -1;

        for (int a : input){
            temp = (a > temp) ? a : temp;
        }

        return temp;
    }
    
</details>
<br>

5. Реализация стека (Stack Implementation)

Реализуйте стек (push, pop, peek, isEmpty) с помощью массива или связанного списка.

<details>
 <summary>Решение 5</summary> 
 </br>

    public class CustomStack {
        private int maxSize;
        private int[] stackArray;
        private int top;

        public CustomStack(int size) {
            this.maxSize = size;
            this.stackArray = new int[maxSize];
            this.top = -1;
        }

        public boolean isFull(){
            return top == maxSize - 1;
        }

        public boolean isEmplty(){
            return top == -1;
        }

        // Метод для добавления элемента в стек (push)
        public void push(int value){
            if (isFull())
                throw new StackOverflowError("Stack is full");

            stackArray[++top] = value;
        }

        // Метод для удаления элемента из стека (pop)
        public int pop(){
            if (isEmplty())
                throw new RuntimeException("Stack is empty");

            return stackArray[top--];
        }

        // Метод для просмотра верхнего элемента стека (peek)
        public int peek(){
            if (isEmplty())
                throw new RuntimeException("Stack is empty");

            return stackArray[top];
        }
    }

    
</details>
<br>

6. Проверка сбалансированности скобок (Balanced Brackets)

Напишите метод, который принимает строку с различными видами скобок (), {}, [], и проверяет, все ли они сбалансированы.

<details>
 <summary>Решение 6</summary> 
 </br>

    public static boolean isBalanced(String input){
        var queue = new ArrayDeque<Character>();

        for (char ch : input.toCharArray()){
            if (ch == '(' || ch == '{' || ch == '[')
                queue.push(ch);
            else {
                if (queue.isEmpty())
                    return false;

                char first = queue.pop();
                if (
                    ch == ')' && first != '(' ||
                    ch == '}' && first != '{' ||
                    ch == ']' && first != '['
                )
                    return false;
            }
        }

        return queue.isEmpty();
    }

    
</details>
<br>

7. Поиск первого уникального символа (First Unique Character)

Напишите функцию, которая находит первый уникальный символ в строке и возвращает его индекс.

<details>
 <summary>Решение 7</summary> 
 </br>

    public static int getFirstUniqueCharacterIndex(String input){
        var charMap = new HashMap<Character, Integer>();

        for (char ch : input.toCharArray()){
            charMap.put(ch, charMap.getOrDefault(ch, 0) + 1);
        }

        for (int i = 0; i < input.length(); i++){
            if (charMap.get(input.charAt(i)) == 1){
                return i;
            }
        }

        return -1;
    }

    
</details>
<br>

8. Нахождение общих элементов в двух массивах (Common Elements in Arrays)

Реализуйте метод, который находит общие элементы между двумя массивами целых чисел int[].

<details>
 <summary>Решение 8</summary> 
 </br>

    public static int[] getCommonElements(int[] a, int[] b){
        var set = new HashSet<Integer>();
        for (int num : a){
            set.add(num);
        }

        var commonElements = new HashSet<Integer>();
        for (int num : b){
            if (set.contains(num))
                commonElements.add(num);
        }

        int[] result = new int[commonElements.size()];
        int i = 0;
        for (int num : commonElements) {
            result[i++] = num;
        }

        return result;
    }

    
</details>
<br>

9. Реализация очереди с двумя стеками (Queue Using Two Stacks)

Реализуйте очередь (enqueue, dequeue) с использованием двух стеков.

<details>
 <summary>Решение 9</summary> 
 </br>

    public class CustomQueue {
        private Stack<Integer> stack1;
        private Stack<Integer> stack2;

        public CustomQueue() {
            this.stack1 = stack1;
            this.stack2 = stack2;
        }

        // Метод для проверки, пуста ли очередь
        public boolean isEmpty() {
            return stack1.isEmpty() && stack2.isEmpty();
        }

        // Метод для добавления элемента в очередь (enqueue)
        public void enqueue(int value) {
            stack1.push(value);
        }

        // Метод для удаления элемента из очереди (dequeue)
        public int dequeue() {
            // Если оба стека пусты, очередь пуста
            if (this.isEmpty()) {
                throw new RuntimeException("Queue is empty");
            }

            // Если stack2 пуст, переносим все элементы из stack1 в stack2
            if (stack2.isEmpty()) {
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }

            // Удаляем элемент из stack2
            return stack2.pop();
        }

        // Метод для просмотра элемента, который находится в начале очереди (peek)
        public int peek() {
            if (this.isEmpty()) {
                throw new RuntimeException("Queue is empty");
            }

            if (stack2.isEmpty()) {
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }

            return stack2.peek();
        }
    }
    
</details>
<br>

10. Нахождение "двойки" (Pair Sum Problem)

Напишите метод, который находит все пары в массиве целых чисел, сумма которых равна заданному числу.

<details>
 <summary>Решение 10</summary> 
 </br>

    public static HashMap<Integer, Integer> findPairs(int[] array, int targetSum){
        HashMap<Integer, Integer> pairMap = new HashMap<>();
        HashSet<Integer> set = new HashSet<>();

        for (int num : array){
            int complement = targetSum - num;

            if (set.contains(complement))
                pairMap.put(num, complement);

            set.add(num);
        }

        return pairMap;
    }

    
</details>
<br>


## 2. Коллекции и работа с данными:

11. Подсчет слов в строке (Word Count)

Напишите метод, который подсчитывает количество слов в строке и выводит их в порядке убывания частоты.

<details>
 <summary>Решение 11</summary> 
 </br>

     public static void countWordFrequencyAndPrintDesc(String input){
        String[] words = input.toLowerCase()
                .replaceAll("[^a-z0-9\\s]", "")
                .split("\\s+");
        HashMap<String, Integer> wordCountMap = new HashMap<>();

        for (String word : words)
            wordCountMap.put(word, wordCountMap.getOrDefault(word, 0) + 1);

        List<Map.Entry<String, Integer>> sortedList = new ArrayList<>(wordCountMap.entrySet());
        
        //  sortedList.sort((entry1, entry2) -> 
            Integer.compare(entry2.getKey().length(), entry1.getKey().length()));
        
        sortedList.sort((entry1, entry2) -> entry2.getValue().compareTo(entry1.getValue()));

        for (Map.Entry<String, Integer> entry : sortedList){
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
    
</details>
<br>

12. Фильтрация списка (Filtering List)

Используя **Stream API**, отфильтруйте список строк, оставив только строки, начинающиеся с определенного символа.

<details>
 <summary>Решение 12</summary> 
 </br>

    public static List<String> filterStringsByChar(List<String> input, char firstChar){
        return input.stream()
                .filter(str -> !str.isEmpty() && str.charAt(0) == firstChar)
                .collect(Collectors.toList());
    }

</details>
<br>

13. Группировка объектов (Grouping Objects)

Используя **Stream API**, сгруппируйте объекты по их определенному полю и подсчитайте их количество в каждой группе.

<details>
 <summary>Решение 13</summary> 
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

14. Поиск максимального значения в списке объектов (Max Value from List of Objects)

Найдите объект с максимальным значением поля в списке, используя **Stream API**.

<details>
 <summary>Решение 14</summary> 
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

19. Реализация LRU-кэша (LRU Cache Implementation)

Реализуйте LRU-кэш (Least Recently Used) с ограничением размера, используя LinkedHashMap или Deque.

<details>
 <summary>Решение 19</summary> 
 </br>

    class LRUCache<K, V> extends LinkedHashMap<K, V> {
        private final int capacity;

        // Конструктор принимает размер кэша
        public LRUCache(int capacity) {
            super(capacity, 0.75f, true);  // true для порядка по доступу (access-order)
            this.capacity = capacity;
        }

        // Переопределяем метод для удаления самого старого элемента при превышении размера
        @Override
        protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
            return size() > capacity;  // Удаление старшего элемента, если размер больше лимита
        }

        // Метод для получения значения из кэша
        public V getCache(K key) {
            return super.getOrDefault(key, null);
        }

        // Метод для добавления значения в кэш
        public void putCache(K key, V value) {
            super.put(key, value);
        }
    }

    // -----

    public static void main(String[] args) {
        // Создаем LRU-кэш с размером 3
        LRUCache<Integer, String> cache = new LRUCache<>(3);

        // Добавляем значения в кэш
        cache.putCache(1, "One");
        cache.putCache(2, "Two");
        cache.putCache(3, "Three");

        // Получаем значение
        System.out.println(cache.getCache(2));  // Two (использовано, теперь оно "свежее")

        // Добавляем новое значение, что вызовет удаление самого старого (1 -> "One")
        cache.putCache(4, "Four");

        // Проверяем, что кэш теперь содержит только последние 3 элемента
        System.out.println(cache.getCache(1));  // null (удалено, так как было самым старым)
        System.out.println(cache.getCache(3));  // Three
        System.out.println(cache.getCache(4));  // Four
    }
    
</details>
<br>

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

## 3. Многопоточность и параллелизм:
21. Создание и запуск потоков (Thread Creation)

Создайте поток, используя как реализацию Runnable, так и расширение Thread, и запустите его.

<details>
 <summary>Решение 21</summary> 
 </br>

    // ---with-Runnable---

    // Реализация интерфейса Runnable
    class MyRunnable implements Runnable {
        @Override
        public void run() {
            System.out.println("Thread using Runnable is running: " + Thread.currentThread().getName());
        }
    }

    public class RunnableExample {
        public static void main(String[] args) {
            // Создание потока с использованием реализации Runnable
            Thread thread1 = new Thread(new MyRunnable());
            
            // Запуск потока
            thread1.start();
        }
    }

    // ---with-Thread---

    // Расширение класса Thread
    class MyThread extends Thread {
        @Override
        public void run() {
            System.out.println("Thread using Thread class is running: " + Thread.currentThread().getName());
        }
    }

    public class ThreadExample {
        public static void main(String[] args) {
            // Создание потока с использованием расширения Thread
            MyThread thread2 = new MyThread();
            
            // Запуск потока
            thread2.start();
        }
    }

    // ---combine-thread---

    public static void main(String[] args) {
        // Поток с реализацией Runnable
        Thread thread1 = new Thread(new MyRunnable());
        
        // Поток с расширением Thread
        MyThread thread2 = new MyThread();

        // Запуск потоков
        thread1.start();
        thread2.start();
    }
    
</details>
<br>

22. Синхронизация потоков (Thread Synchronization)

Напишите пример синхронизации доступа к общему ресурсу между несколькими потоками с использованием ключевого слова synchronized.

<details>
 <summary>Решение 22</summary> 
 </br>

    class Counter {
        private int count = 0;

        // Синхронизированный метод для увеличения счетчика
        public synchronized void increment() {
            count++;
        }

        // Получение текущего значения счетчика
        public int getCount() {
            return count;
        }
    }

    class CounterThread extends Thread {
        private Counter counter;

        // Конструктор, принимающий общий объект Counter
        public CounterThread(Counter counter) {
            this.counter = counter;
        }

        @Override
        public void run() {
            for (int i = 0; i < 1000; i++) {
                counter.increment();  // Увеличиваем счетчик
            }
        }
    }

    public static void main(String[] args) {
        Counter counter = new Counter();  // Общий ресурс

        // Создаем несколько потоков, которые будут работать с одним и тем же объектом Counter
        CounterThread thread1 = new CounterThread(counter);
        CounterThread thread2 = new CounterThread(counter);
        CounterThread thread3 = new CounterThread(counter);

        // Запускаем потоки
        thread1.start();
        thread2.start();
        thread3.start();

        // Ждем, пока потоки завершат выполнение
        try {
            thread1.join();
            thread2.join();
            thread3.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // Выводим итоговое значение счетчика
        System.out.println("Final count: " + counter.getCount());
    }
    
</details>
<br>

23. Использование ExecutorService для управления потоками (Thread Pool)

Создайте ExecutorService и выполните несколько задач параллельно, используя Callable.

<details>
 <summary>Решение 23</summary> 
 </br>
    public static void main(String[] args) {
        // Создаем пул потоков с фиксированным количеством потоков (3 потока)
        ExecutorService executorService = Executors.newFixedThreadPool(3);

        // Список задач Callable
        List<Callable<Integer>> taskList = new ArrayList<>();

        // Добавляем задачи в список
        for (int i = 1; i <= 5; i++) {
            int taskId = i;
            taskList.add(() -> {
                System.out.println("Task " + taskId + " is running in " + Thread.currentThread().getName());
                return taskId * 10;  // Возвращаем результат выполнения задачи
            });
        }

        try {
            // Выполняем задачи параллельно и получаем список объектов Future
            List<Future<Integer>> futures = executorService.invokeAll(taskList);

            // Получаем результаты выполнения всех задач
            for (Future<Integer> future : futures) {
                try {
                    System.out.println("Result: " + future.get());  // Получение результата выполнения задачи
                } catch (ExecutionException | InterruptedException e) {
                    e.printStackTrace();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // Завершаем работу ExecutorService
            executorService.shutdown();
        }
    }
    
</details>
<br>

24. Проблема Producer-Consumer (Producer-Consumer Problem)

Реализуйте классический пример проблемы "производитель-потребитель" с использованием блокирующей очереди (BlockingQueue).

<details>
 <summary>Решение 24</summary> 
 </br>

    class Producer implements Runnable {
        private final BlockingQueue<Integer> queue;

        public Producer(BlockingQueue<Integer> queue) {
            this.queue = queue;
        }

        @Override
        public void run() {
            int value = 0;
            try {
                while (true) {
                    System.out.println("Producer produced: " + value);
                    queue.put(value);  // Добавляем элемент в очередь, блокируется, если очередь полна
                    value++;
                    Thread.sleep(1000);  // Симуляция времени производства
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    class Consumer implements Runnable {
        private final BlockingQueue<Integer> queue;

        public Consumer(BlockingQueue<Integer> queue) {
            this.queue = queue;
        }

        @Override
        public void run() {
            try {
                while (true) {
                    int value = queue.take();  // Извлекаем элемент из очереди, блокируется, если очередь пуста
                    System.out.println("Consumer consumed: " + value);
                    Thread.sleep(1500);  // Симуляция времени потребления
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    public class ProducerConsumerExample {
        public static void main(String[] args) {
            // Создаем блокирующую очередь с максимальной емкостью 5
            BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

            // Создаем и запускаем потоки производителя и потребителя
            Thread producerThread = new Thread(new Producer(queue));
            Thread consumerThread = new Thread(new Consumer(queue));

            producerThread.start();
            consumerThread.start();
        }
    }
    
</details>
<br>

25. Реализация CountDownLatch (CountDownLatch Usage)

Используйте CountDownLatch для ожидания завершения нескольких потоков перед выполнением основного потока.

<details>
 <summary>Решение 25</summary> 
 </br>

    class Worker implements Runnable {
        private final CountDownLatch latch;
        private final int workerId;

        public Worker(int workerId, CountDownLatch latch) {
            this.workerId = workerId;
            this.latch = latch;
        }

        @Override
        public void run() {
            try {
                // Симулируем выполнение задачи
                System.out.println("Worker " + workerId + " is doing work.");
                Thread.sleep(2000); // Симуляция работы
                System.out.println("Worker " + workerId + " has finished work.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                // Уменьшаем счетчик на 1, когда поток завершает свою работу
                latch.countDown();
            }
        }
    }

    public class CountDownLatchExample {
        public static void main(String[] args) {
            // Инициализируем CountDownLatch для 3 потоков
            CountDownLatch latch = new CountDownLatch(3);

            // Создаем и запускаем 3 рабочих потока
            for (int i = 1; i <= 3; i++) {
                new Thread(new Worker(i, latch)).start();
            }

            try {
                // Основной поток ждет, пока все рабочие потоки завершат свою работу
                latch.await();
                System.out.println("All workers have finished their tasks. Proceeding with main thread.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
        
</details>
<br>

26. Проблема гонки (Race Condition)

Создайте пример, демонстрирующий проблему гонки (Race Condition) и покажите, как ее решить с помощью синхронизации.

<details>
 <summary>Решение 26</summary> 
 </br>

    class Counter {
        private int count = 0;

        public void increment() {
            count++;
        }

        public int getCount() {
            return count;
        }
    }

    class Worker implements Runnable {
        private final Counter counter;

        public Worker(Counter counter) {
            this.counter = counter;
        }

        @Override
        public void run() {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        }
    }

    public class RaceConditionExample {
        public static void main(String[] args) throws InterruptedException {
            Counter counter = new Counter(); // Общий ресурс (счетчик)
            
            // Создаем 3 потока, которые будут увеличивать счетчик
            Thread t1 = new Thread(new Worker(counter));
            Thread t2 = new Thread(new Worker(counter));
            Thread t3 = new Thread(new Worker(counter));

            // Запускаем потоки
            t1.start();
            t2.start();
            t3.start();

            // Ждем завершения всех потоков
            t1.join();
            t2.join();
            t3.join();

            // Ожидаемое значение счетчика: 3000 (3 потока по 1000 инкрементов)
            System.out.println("Final count (without synchronization): " + counter.getCount());
        }
    }
    
</details>
<br>

27. Использование CompletableFuture для асинхронных операций (CompletableFuture Usage)

Напишите пример, где выполняется асинхронная операция с использованием CompletableFuture.

<details>
 <summary>Решение 27</summary> 
 </br>

    public class CompletableFutureExample {
        public static void main(String[] args) {
            // Асинхронная задача для имитации загрузки данных
            CompletableFuture<String> dataFuture = CompletableFuture.supplyAsync(() -> {
                System.out.println("Loading data in: " + Thread.currentThread().getName());
                try {
                    // Симуляция задержки для имитации длительной задачи
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                return "Data loaded successfully";
            });

            // Обработка результата после завершения загрузки данных
            CompletableFuture<String> processedDataFuture = dataFuture.thenApply(data -> {
                System.out.println("Processing data in: " + Thread.currentThread().getName());
                return data + " and processed";
            });

            // Печать результата после обработки данных
            processedDataFuture.thenAccept(result -> {
                System.out.println("Result: " + result);
            });

            // Ждем завершения всех операций
            try {
                // Получаем окончательный результат для блокирующей части программы (если нужно)
                processedDataFuture.get();  // Ждем завершения всех операций
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }

            System.out.println("Main thread continues...");
        }
    }
    
</details>
<br>

28. Реализация Semaphore для ограничения доступа к ресурсу (Semaphore Usage)

Используйте Semaphore, чтобы ограничить доступ к ресурсу для определенного количества потоков.

<details>
 <summary>Решение 28</summary> 
 </br>

    class SharedResource {
        private final Semaphore semaphore;

        // Инициализация Semaphore с определенным количеством "доступов" (например, 2)
        public SharedResource(int permits) {
            this.semaphore = new Semaphore(permits);
        }

        // Метод, симулирующий доступ к ресурсу
        public void accessResource(String threadName) {
            try {
                System.out.println(threadName + " is trying to access the resource.");
                // Получаем доступ (или блокируемся, если доступов недостаточно)
                semaphore.acquire();

                System.out.println(threadName + " has acquired the resource.");
                // Симулируем работу с ресурсом
                Thread.sleep(2000);

                System.out.println(threadName + " is releasing the resource.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                // Освобождаем доступ к ресурсу
                semaphore.release();
            }
        }
    }

    class Worker extends Thread {
        private final SharedResource resource;

        public Worker(String name, SharedResource resource) {
            super(name);
            this.resource = resource;
        }

        @Override
        public void run() {
            resource.accessResource(getName());
        }
    }

    public class SemaphoreExample {
        public static void main(String[] args) {
            // Создаем общий ресурс с ограничением на 2 потока, которые могут его использовать одновременно
            SharedResource resource = new SharedResource(2);

            // Запускаем 5 потоков, которые будут пытаться получить доступ к ресурсу
            Worker worker1 = new Worker("Worker 1", resource);
            Worker worker2 = new Worker("Worker 2", resource);
            Worker worker3 = new Worker("Worker 3", resource);
            Worker worker4 = new Worker("Worker 4", resource);
            Worker worker5 = new Worker("Worker 5", resource);

            worker1.start();
            worker2.start();
            worker3.start();
            worker4.start();
            worker5.start();
        }
    }
    
</details>
<br>

29. Реализация потока с периодическим выполнением задачи (ScheduledExecutorService)

Используйте ScheduledExecutorService для выполнения задачи через фиксированные интервалы времени.

<details>
 <summary>Решение 29</summary> 
 </br>

    public class ScheduledExecutorExample {

        public static void main(String[] args) {
            // Создаем ScheduledExecutorService с пулом потоков на 1 поток
            ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);

            // Задача, которая будет выполняться через фиксированные интервалы времени
            Runnable task = () -> {
                System.out.println("Task is running at: " + System.currentTimeMillis());
            };

            // Запланировать выполнение задачи через каждые 3 секунды, начиная с задержки в 1 секунду
            scheduler.scheduleAtFixedRate(task, 1, 3, TimeUnit.SECONDS);

            // Ждем некоторое время, чтобы продемонстрировать выполнение задачи, и затем завершаем работу
            try {
                Thread.sleep(15000);  // Даем задаче выполниться несколько раз
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            // Завершаем выполнение scheduler
            scheduler.shutdown();
        }
    }
    
</details>
<br>

## 4. Общие задачи по OOP и лучшим практикам:
30. Создание иерархии классов (Inheritance & Polymorphism)

Создайте базовый класс и несколько его подклассов, чтобы продемонстрировать полиморфизм, наследование и переопределение методов.

<details>
 <summary>Решение 30</summary> 
 </br>

    // Базовый класс
    class Animal {
        // Метод, который будет переопределен в подклассах
        public void makeSound() {
            System.out.println("The animal makes a sound.");
        }

        // Общий метод для всех животных
        public void eat() {
            System.out.println("The animal is eating.");
        }
    }

    // Подкласс Dog, который наследует от Animal
    class Dog extends Animal {
        // Переопределяем метод makeSound() для класса Dog
        @Override
        public void makeSound() {
            System.out.println("The dog barks.");
        }
    }

    // Подкласс Cat, который наследует от Animal
    class Cat extends Animal {
        // Переопределяем метод makeSound() для класса Cat
        @Override
        public void makeSound() {
            System.out.println("The cat meows.");
        }
    }

    public class PolymorphismExample {
        public static void main(String[] args) {
            // Использование полиморфизма: объект типа Animal может ссылаться на подклассы Dog и Cat
            Animal myDog = new Dog();
            Animal myCat = new Cat();

            // Вызов метода makeSound() для каждого объекта
            // Метод, который будет вызван, зависит от реального типа объекта
            myDog.makeSound();  // Вызов метода из класса Dog
            myCat.makeSound();  // Вызов метода из класса Cat

            // Вызов общего метода eat() для обоих объектов
            myDog.eat();  // Метод из класса Animal
            myCat.eat();  // Метод из класса Animal
        }
    }
    
</details>
<br>
