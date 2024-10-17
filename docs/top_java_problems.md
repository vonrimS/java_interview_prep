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

    public class CustomQueue<T> {
        private Stack<T> stack1;  // для добавления элементов (enqueue)
        private Stack<T> stack2;  // для удаления элементов (dequeue)

        public CustomQueue(Stack<T> stack1, Stack<T> stack2) {
            this.stack1 = stack1;
            this.stack2 = stack2;
        }

        // Метод для добавления элемента в очередь (enqueue)
        public void enqueue(T item) {
            stack1.push(item);
        }
        
        // Метод для удаления элемента из очереди (dequeue)
        public T dequeue () {
            if (stack2.isEmpty()) {
                // Перекладываем элементы из stack1 в stack2
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }

            // Если stack2 пустой, значит очередь пуста
            if (stack2.isEmpty()) {
                throw new RuntimeException("Queue is empty");
            }

            // Удаляем элемент из stack2
            return stack2.pop();
        }

        // Метод для проверки, пуста ли очередь
        public boolean isEmpty () {
            return stack1.isEmpty() && stack2.isEmpty();
        }

        // Метод для получения первого элемента без его удаления (peek)
        public T peek () {
            if (stack2.isEmpty()) {
                // Перекладываем элементы из stack1 в stack2
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }

            // Если очередь пуста
            if (stack2.isEmpty()) {
                throw new RuntimeException("Queue is empty");
            }

            // Возвращаем первый элемент без удаления
            return stack2.peek();
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
        // подготовить строку и разобрать
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
 <summary>Решение 14</summary> 
 </br>
 
    public static int fibonacci(int n) {
        if (n <= 1) {
            return n;
        } else {
            return fibonacci(n - 1) + fibonacci(n - 2);
        }
    }

</details>
<br>


15. **Ханойские башни**

Задача на Ханойские башни заключается в перемещении `n` дисков с одного стержня на другой с помощью промежуточного стержня, следуя правилам: **можно перемещать только один диск за раз, и нельзя класть больший диск на меньший**.

Напишите метод solveHanoi(int n, char fromRod, char toRod, char auxRod), который рекурсивно решает задачу Ханойских башен.

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
