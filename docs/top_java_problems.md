# Частые `java core` задачи

1. **Реверс строки (String Reversal)**

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


2. **Проверка палиндрома (Palindrome Check)**

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

3. **Анаграммы (Anagram Check)**

Напишите метод, который определяет, являются ли две строки анаграммами (имеют одинаковые символы в разном порядке).
<details>
 <summary>Решение 3</summary> 
 </br>

    public static boolean isAnagram(String a, String b){
        if (a.length() != b.length())
            return false;

        Map<Character, Integer> charMap = new HashMap<>();

        for (int i = 0; i < a.length(); i++){
            char c = a.charAt(i);
            charMap.put(c, charMap.getOrDefault(c, 0) + 1);
        }

        for (int i = 0; i < b.length(); i++){
            char c = b.charAt(i);
            if (!charMap.containsKey(c))
                return false; // символ 'b' не найден в 'a'

            charMap.put(c, charMap.get(c) - 1);
            if (charMap.get(c) == 0)
                charMap.remove(c);
        }

        return charMap.isEmpty();
    }
    
</details>
<br>

4. **Найти наибольший элемент в массиве (Find Max Element)**

Напишите функцию, которая находит наибольший элемент в массиве целых чисел.

<details>
 <summary>Решение 4</summary> 
 </br>

    public static int getBiggestElement(int[] input){
        int temp = Integer.MIN_VALUE;;

        for (int a : input){
            temp = (a > temp) ? a : temp;
        }

        return temp;
    }
    
</details>
<br>

5. **Реализация стека (Stack Implementation)**

Реализуйте стек (`push`, `pop`, `peek`, `isEmpty`, `isFull`) с помощью массива или связанного списка.
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


6. **Проверка сбалансированности скобок (Balanced Brackets)**

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


7. **Поиск первого уникального символа (First Unique Character)**

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


8. **Нахождение общих элементов в двух массивах (Common Elements in Arrays)**

Реализуйте метод, который находит общие элементы между двумя массивами целых чисел `int[]`.

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

9. **Реализация очереди с двумя стеками (Queue Using Two Stacks)**

Реализуйте очередь с методами `enqueue`, `dequeue`, `isFull`, `isEmpty` с использованием двух стеков.
<details>
 <summary>Решение 9</summary> 
 </br>

    public class CustomQueueWithTwoStacks<T> {
        private Stack<T> stack1;  // для вставки элементов - enqueue
        private Stack<T> stack2;  // для извлечения элементов - dequeue
        private int maxSize;      // максимальный размер очереди

        public CustomQueueWithTwoStacks(int maxSize) {
            this.maxSize = maxSize;
            this.stack1 = new Stack<>();
            this.stack2 = new Stack<>();
        }

        public boolean isEmpty(){
            return stack1.isEmpty() && stack2.isEmpty();
        }

        public boolean isFull(){
            return stack1.size() + stack2.size() == maxSize;
        }

        public void enqueue(T item){
            if (isFull())
                throw new RuntimeException("Queue is full");

            stack1.push(item);
        }

        public T dequeue(){
            if (isEmpty())
                throw new RuntimeException("Queue is empty");

            if (stack2.isEmpty()){
                while (!stack1.isEmpty()){
                    stack2.push(stack1.pop());
                }
            }

            return stack2.pop();
        }

    }
    
</details>
<br>


10. **Нахождение "двойки" (Pair Sum Problem)**

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


11. **Подсчет слов в строке (Word Count)**

Напишите метод, который подсчитывает количество слов в строке и выводит их в порядке убывания частоты.

<details>
 <summary>Решение 11</summary> 
 </br>

     public static void countWordFrequencyAndPrintDesc(String input){
        // удаляет все не буквы и не цифры, разбивает на массив по пробелам любой длины между словами
        String[] words = input.toLowerCase().replaceAll("[^a-z0-9\\s]", "").split("\\s+");

        var map = new HashMap<String, Integer>();

        for (String word : words)
            map.put(word, map.getOrDefault(word, 0) + 1);

        List<Map.Entry<String, Integer>> sortedList = new ArrayList<>(map.entrySet());

        // сортирует по длине строки и алфавиту если слова одинаковые по длине
        sortedList.sort((e1, e2) -> Integer.compare(e2.getKey().length(), e1.getKey().length()));

        // сортирует по количеству повторов в стоке
        sortedList.sort((e1, e2) -> e2.getValue().compareTo(e1.getValue()));

        for (Map.Entry<String, Integer> entry : sortedList)
            System.out.println(entry.getKey() + " : " + entry.getValue());
    }
    
</details>
<br>

12. **Создание иерархии классов, `Inheritance` и `Polymorphism`**

Создайте базовый класс и несколько его подклассов, чтобы продемонстрировать полиморфизм, наследование и переопределение методов.

<details>
 <summary>Решение 12</summary> 
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


13. **Вычисление факториала**

Напишите метод `factorial(int n)`, который возвращает факториал числа `n`.

<details>
 <summary>Пример факториала</summary> 
 </br>
    1! = 1;
    2! = 2;
    3! = 6;
    4! = 24;
    5! = 120;
    6! = 720;
</details>


<details>
 <summary>Решение 13</summary> 
 </br>
  
    public static int factorial(int n) {
        if (n == 0 || n == 1) {
            return 1;
        } else {
            return n * factorial(n - 1);
        }
    }

</details>
<br>


14. **Числа Фибоначчи**

Напишите метод `fibonacci(int n)`, который возвращает `n`-е число Фибоначчи.

<details>
 <summary>Последовательность Фибоначчи</summary> 
 </br>

    0 -> 0
    1 -> 1
    2 -> 1
    3 -> 2
    4 -> 3
    5 -> 5
    6 -> 8
    7 -> 13
    8 -> 21
    9 -> 34
    10 -> 55

</details>


<details>
 <summary>Решение 14</summary> 
 </br>
 
    // рекурсивный подход
    public static int getFibonacci(int n){
        if (n <= 1)
            return n;
        else
            return getFibonacci(n - 1) + getFibonacci(n - 2);
    }

    // итеративный подход
    public static int getFibonacciIterative(int n){
        if (n <= 1)
            return n;

        int prev = 0, curr = 1;

        for (int i = 2; i <= n; i++){
            int next = prev + curr;
            prev = curr;
            curr = next;
        }

        return curr;
    }

</details>
<br>


15. **Ханойские башни**

Задача на Ханойские башни заключается в перемещении `n` дисков с одного стержня на другой с помощью промежуточного стержня, следуя правилам: **можно перемещать только один диск за раз, и нельзя класть больший диск на меньший**.

Напишите метод `solveHanoi(int n, char fromRod, char toRod, char auxRod)`, который рекурсивно решает задачу Ханойских башен.

<details>
 <summary>Решение 15</summary> 
 </br>

    public static void solveHanoi(int n, char fromRod, char toRod, char auxRod) {
        if (n == 1) {
            System.out.println("Move disk 1 from rod " + fromRod + " to rod " + toRod);
            return;
        }
        solveHanoi(n - 1, fromRod, auxRod, toRod);
        System.out.println("Move disk " + n + " from rod " + fromRod + " to rod " + toRod);
        solveHanoi(n - 1, auxRod, toRod, fromRod);
    }

</details>
<br>

16. **FizzBuzz**

Задача `FizzBuzz` заключается в том, чтобы вывести числа от `1` до `N`. Но:

Если число делится на 3, выведите `Fizz`.
Если число делится на 5, выведите `Buzz`.
Если число делится на 3 и на 5, выведите `FizzBuzz`.

<details>
 <summary>Решение 16</summary> 
 </br>

    public static void fizzBuzz(int n) {
        for (int i = 1; i <= n; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                System.out.println("FizzBuzz");
            } else if (i % 3 == 0) {
                System.out.println("Fizz");
            } else if (i % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(i);
            }
        }
    }

</details>
<br>


17. **Рекурсия: Возведение числа в степень**

Напишите метод `power(int base, int exp)`, который возвращает результат возведения числа `base` в степень `exp`.

<details>
 <summary>Решение 17</summary> 
 </br>

    public static int power(int base, int exp) {
        if (exp == 0) {
            return 1;
        } else {
            return base * power(base, exp - 1);
        }
    }

</details>
<br>


18. **Определение суммы цифр натурального числа**

Напишите метод, который принимает на вход натуральное число (целое положительное число) и возвращает сумму его цифр.

<details>
 <summary>Решение 18</summary> 
 </br>
  
    // Метод для вычисления суммы цифр числа
    public static int sumOfDigits(int number) {
        int sum = 0;
        
        // Цикл для получения суммы цифр числа
        while (number > 0) {
            sum += number % 10; // Добавляем последнюю цифру
            number /= 10;       // Убираем последнюю цифру
        }
        
        return sum;
    }

</details>
<br>

19. **Рекурсивная функция для вычисления суммы элементов массива**

Напишите рекурсивную функцию, которая принимает массив целых чисел и возвращает их сумму.

<details>
 <summary>Решение 19</summary> 
 </br>
  
    public static int sum(int[] arr, int n) {
        if (n == 0) return 0;
        return arr[n - 1] + sum(arr, n - 1);
    }

</details>
<br>

20. **`FizzBuzz` с дополнительной логикой**

Модифицируйте классическую задачу `FizzBuzz` так, чтобы она выводила:

* "Fizz" для чисел, кратных 3
* "Buzz" для чисел, кратных 5
* "FizzBuzz" для чисел, кратных и 3, и 5
* "BuzzFizz" для чисел, кратных 7, но не 3 и 5

<details>
 <summary>Решение 20</summary> 
 </br>
  
    public static void fizzBuzz(int n) {
        for (int i = 1; i <= n; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                System.out.println("FizzBuzz");
            } else if (i % 7 == 0) {
                System.out.println("BuzzFizz");
            } else if (i % 3 == 0) {
                System.out.println("Fizz");
            } else if (i % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(i);
            }
        }
    }

</details>
<br>

21. **Поиск пути в лабиринте (`Backtracking`)**

Напишите программу, которая решает задачу поиска пути в лабиринте с использованием метода поиска с возвратом (`backtracking`). Лабиринт представлен `2D` массивом, где

* `1` — это путь
* `0` — это стена

<details>
 <summary>Решение 21</summary> 
 </br>

    public static boolean solveMaze(int[][] maze, int x, int y) {
        if (x == maze.length - 1 && y == maze[0].length - 1)
            return true;
        
        if (x >= 0 && x < maze.length && y >= 0 && y < maze[0].
        length && maze[x][y] == 1) {
            maze[x][y] = 2;  // Маркируем как посещенный
            if (solveMaze(maze, x + 1, y)) return true;  // Вниз
            if (solveMaze(maze, x, y + 1)) return true;  // Вправо
            if (solveMaze(maze, x - 1, y)) return true;  // Вверх
            if (solveMaze(maze, x, y - 1)) return true;  // Влево
            maze[x][y] = 1;  // Отмена хода
        }

        return false;
    }

</details>
<br>

22. **Реализация `Singleton` с ленивой инициализацией**

Напишите потокобезопасную реализацию `Singleton` с ленивой инициализацией.

<details>
 <summary>Решение 22</summary> 
 </br>
    public class Singleton {
        private static Singleton instance;
        
        private Singleton() {}
        
        public static Singleton getInstance() {
            if (instance == null) {
                synchronized (Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }

</details>
<br>


23. **Алгоритм Левенштейна (Редакционное расстояние)**

Расстояние Левенштейна, или редакционное расстояние, — метрика cходства между двумя строковыми последовательностями.

Реализуйте рекурсивную (или динамическую) версию вычисления редакционного расстояния между двумя строками.

<details>
 <summary>Решение 23</summary> 
 </br>
  
    public static int levenshtein(String s1, String s2) {
        if (s1.isEmpty()) return s2.length();
        if (s2.isEmpty()) return s1.length();
        
        int cost = s1.charAt(0) == s2.charAt(0) ? 0 : 1;
        
        return Math.min(
            Math.min(levenshtein(s1.substring(1), s2) + 1, 
                    levenshtein(s1, s2.substring(1)) + 1), 
                    levenshtein(s1.substring(1), s2.substring(1)) + cost);
    }

</details>
<br>

24. **Реализация собственной `HashMap`**

Напишите метод, который принимает на вход натуральное число (целое положительное число) и возвращает сумму его цифр.

<details>
 <summary>Решение 24</summary> 
 </br>
  
    public class MyHashMap<K, V> {
        private class Node<K, V> {
            K key;
            V value;
            Node<K, V> next;
            // Конструктор и методы
        }

        private Node<K, V>[] buckets;
        private int capacity;
        
        public MyHashMap() {
            capacity = 16;
            buckets = new Node[capacity];
        }
        
        public void put(K key, V value) {
            int index = key.hashCode() % capacity;
            Node<K, V> newNode = new Node<>(key, value, null);
            // Логика вставки
        }
        
        public V get(K key) {
            int index = key.hashCode() % capacity;
            // Логика поиска
        }
    }

</details>
<br>


25. **Переворот односвязного списка**

Напишите функцию для переворота односвязного списка. Классическая задача для работы с указателями.

<details>
 <summary>Решение 25</summary> 
 </br>
  
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        return prev;
    }

</details>
<br>


26. **Определение цикла в односвязном списке**

Используя алгоритм Флойда (алгоритм черепахи и зайца), определите, содержит ли односвязный список цикл.

<details>
 <summary>Решение 26</summary> 
 </br>

    public static boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }

</details>
<br>
