# Топ-20 `Java` задач по `Collections`

1. **Реализуйте собственную HashMap**

Создайте свою реализацию `HashMap` с использованием открытой адресации или цепочек для разрешения коллизий.
Решение:

java
Copy code
class CustomHashMap<K, V> {
    private class Entry<K, V> {
        K key;
        V value;
        Entry<K, V> next;

        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int SIZE = 16;
    private Entry<K, V>[] table;

    public CustomHashMap() {
        table = new Entry[SIZE];
    }

    public void put(K key, V value) {
        int index = key.hashCode() % SIZE;
        Entry<K, V> entry = new Entry<>(key, value);

        if (table[index] == null) {
            table[index] = entry;
        } else {
            Entry<K, V> current = table[index];
            while (current.next != null) {
                current = current.next;
            }
            current.next = entry;
        }
    }

    public V get(K key) {
        int index = key.hashCode() % SIZE;
        Entry<K, V> current = table[index];
        while (current != null) {
            if (current.key.equals(key)) {
                return current.value;
            }
            current = current.next;
        }
        return null;
    }
}
2. Топ-k часто встречающихся элементов в списке:
Описание: Используйте PriorityQueue для нахождения k наиболее частых элементов в списке чисел.
Решение:

java
Copy code
Map<Integer, Integer> frequencyMap = new HashMap<>();
for (int num : list) {
    frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
}

PriorityQueue<Map.Entry<Integer, Integer>> queue = new PriorityQueue<>(
    (a, b) -> b.getValue() - a.getValue()
);

queue.addAll(frequencyMap.entrySet());
3. Реализуйте очередь с приоритетом с использованием Heap:
Описание: Создайте собственную реализацию очереди с приоритетом (Priority Queue) с использованием кучи.
Решение:

java
Copy code
class PriorityQueueHeap {
    private int[] heap;
    private int size;
    
    public PriorityQueueHeap(int capacity) {
        heap = new int[capacity];
    }
    
    public void insert(int value) {
        if (size == heap.length) throw new IllegalStateException();
        heap[size] = value;
        size++;
        heapifyUp(size - 1);
    }

    public int poll() {
        if (size == 0) throw new IllegalStateException();
        int root = heap[0];
        heap[0] = heap[size - 1];
        size--;
        heapifyDown(0);
        return root;
    }

    private void heapifyUp(int index) {
        while (index > 0 && heap[index] > heap[parentIndex(index)]) {
            swap(index, parentIndex(index));
            index = parentIndex(index);
        }
    }

    private void heapifyDown(int index) {
        while (leftChildIndex(index) < size) {
            int largerChildIndex = leftChildIndex(index);
            if (rightChildIndex(index) < size && heap[rightChildIndex(index)] > heap[largerChildIndex]) {
                largerChildIndex = rightChildIndex(index);
            }
            if (heap[index] >= heap[largerChildIndex]) break;
            swap(index, largerChildIndex);
            index = largerChildIndex;
        }
    }

    private int parentIndex(int index) { return (index - 1) / 2; }
    private int leftChildIndex(int index) { return 2 * index + 1; }
    private int rightChildIndex(int index) { return 2 * index + 2; }
    private void swap(int i, int j) { int temp = heap[i]; heap[i] = heap[j]; heap[j] = temp; }
}
4. Найдите медиану в потоке данных с использованием PriorityQueue:
Описание: Используйте две очереди с приоритетом для нахождения медианы в динамически поступающих данных.
Решение:

java
Copy code
class MedianFinder {
    private PriorityQueue<Integer> low = new PriorityQueue<>(Collections.reverseOrder());
    private PriorityQueue<Integer> high = new PriorityQueue<>();

    public void addNum(int num) {
        low.offer(num);
        high.offer(low.poll());

        if (low.size() < high.size()) {
            low.offer(high.poll());
        }
    }

    public double findMedian() {
        if (low.size() > high.size()) {
            return low.peek();
        }
        return (low.peek() + high.peek()) / 2.0;
    }
}
5. Реализуйте пропускную таблицу (Skip List):
Описание: Реализуйте структуру данных Skip List, используемую для поиска элементов с O(log n) сложностью.
6. Слияние k отсортированных списков:
Описание: Используйте PriorityQueue для слияния k отсортированных списков в один отсортированный список.
7. Реализуйте двусвязный список с поддержкой LRU-кэша:
Описание: Реализуйте двусвязный список для хранения элементов в порядке доступа и используйте его для реализации LRU-кэша с O(1) временем доступа.

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



8. Нахождение пересечения двух списков:
Описание: Напишите метод, который принимает два списка и возвращает новый список, содержащий только те элементы, которые встречаются в обоих списках.
Решение:

java
Copy code
public List<Integer> findIntersection(List<Integer> list1, List<Integer> list2) {
    List<Integer> result = new ArrayList<>();
    Set<Integer> set = new HashSet<>(list1);
    for (int num : list2) {
        if (set.contains(num)) {
            result.add(num);
        }
    }
    return result;
}
9. Удаление дубликатов из ArrayList:
Описание: Напишите метод, который удаляет дубликаты из ArrayList<Integer> без использования Set.
Решение:

java
Copy code
public List<Integer> removeDuplicates(List<Integer> list) {
    List<Integer> result = new ArrayList<>();
    for (Integer num : list) {
        if (!result.contains(num)) {
            result.add(num);
        }
    }
    return result;
}
10. Реализуйте метод для проверки баланса скобок с использованием стека:
Описание: Реализуйте алгоритм для проверки, сбалансированы ли строки с различными типами скобок.
Решение:

java
Copy code
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[') {
            stack.push(c);
        } else {
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            if (c == ')' && top != '(' || c == '}' && top != '{' || c == ']' && top != '[') return false;
        }
    }
    return stack.isEmpty();
}
11. Нахождение всех подмножеств множества (power set):
Описание: Напишите функцию для нахождения всех подмножеств (power set) множества, используя коллекции.
12. Реализуйте красно-черное дерево (Red-Black Tree):
Описание: Создайте собственную реализацию красно-черного дерева с поддержкой операций вставки, удаления и балансировки.
13. Реализуйте алгоритм на основе Trie для поиска строк:
Описание: Реализуйте структуру данных Trie для поиска префиксов в множестве строк.
14. Напишите алгоритм Дейкстры с использованием PriorityQueue:
Описание: Реализуйте алгоритм поиска кратчайшего пути в графе с использованием PriorityQueue.
15. Реализуйте топологическую сортировку на графе:
Описание: Реализуйте топологическую сортировку направленного ациклического графа (DAG) с использованием Stack.
16. Реализуйте метод для детекции цикла в связанном списке:
Описание: Реализуйте алгоритм для проверки наличия цикла в односвязном списке с использованием двух указателей.
17. Перевод Map в List и обратно:
Описание: Напишите метод, который преобразует Map<Integer, String> в List<Integer> и обратно.
18. Удаление элементов из List по условию:
Описание: Напишите метод, который удаляет все элементы из ArrayList<String>, которые содержат определённую подстроку.
19. Проверка, содержится ли один список в другом:
Описание: Напишите метод, который проверяет, содержится ли один список как подсписок внутри другого списка.
20. Проверка уникальности элементов в массиве:
Описание: Напишите метод, который проверяет, содержатся ли все элементы массива уникальными, используя только List и Set.




7. Напишите функцию для сортировки большого файла с числами:
Создайте алгоритм для сортировки файла, который не помещается в память, используя External Sort и структуры данных для работы с потоками.

8. Реализуйте механизм автоматического удаления "устаревших" элементов в коллекции:
Используйте ConcurrentHashMap с ScheduledExecutorService, чтобы автоматически удалять элементы через определенное время после их добавления.

java
Copy code
ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
9. Напишите реализацию бин-поиска в отсортированном ArrayList:
Реализуйте бинарный поиск в ArrayList с соблюдением инвариантов и логики работы поиска в списках.

java
Copy code
Collections.binarySearch(list, key);
10. Реализуйте собственный ConcurrentHashMap:
Создайте упрощенную версию ConcurrentHashMap, которая поддерживает блокировку сегментов (segmented locks) для повышения производительности.

11. Реализуйте двусвязный список с поддержкой LRU-кэша:
Создайте двусвязный список для хранения элементов в порядке доступа, и используйте его для реализации LRU-кэша с O(1) временем доступа и удаления.

java
Copy code
class LRUCache<K, V> extends LinkedHashMap<K, V> {
    // ...
}
12. Напишите алгоритм для проверки баланса скобок с использованием стека:
Реализуйте алгоритм для проверки, сбалансированы ли строки с различными типами скобок, используя структуру данных Stack.

java
Copy code
Stack<Character> stack = new Stack<>();
13. Найдите все подмножества заданного множества (power set):
Напишите функцию для нахождения всех подмножеств (power set) заданного множества, используя коллекции.

java
Copy code
Set<Set<Integer>> powerSet = new HashSet<>();
14. Реализуйте красно-черное дерево (Red-Black Tree):
Создайте собственную реализацию красно-черного дерева с поддержкой основных операций вставки, удаления и балансировки.

15. Реализуйте алгоритм на основе Trie для поиска строк:
Создайте реализацию дерева Trie для поиска префиксов в большом множестве строк.

java
Copy code
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isWord = false;
}
16. Напишите функцию для проверки, является ли граф двусвязным, используя BFS/DFS:
Используйте граф, реализованный на основе списков смежности, и примените BFS или DFS для проверки, является ли граф двусвязным.

17. Реализуйте алгоритм Дейкстры с использованием PriorityQueue:
Реализуйте алгоритм поиска кратчайшего пути в графе с использованием очереди с приоритетом и карты смежностей.

18. Реализуйте алгоритм топологической сортировки на графе:
Реализуйте топологическую сортировку направленного ациклического графа (DAG) с использованием Stack и алгоритмов обхода графов.

19. Напишите реализацию слияния интервалов (merge intervals):
Дана коллекция интервалов, объедините все пересекающиеся интервалы и верните результат в виде списка непересекающихся интервалов.

java
Copy code
Collections.sort(intervals, Comparator.comparingInt(a -> a.start));
20. Реализуйте метод для детекции цикла в связанном списке:
Реализуйте алгоритм для проверки наличия цикла в односвязном списке с использованием алгоритма "двух указателей" (Floyd's Tortoise and Hare).

java
Copy code
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}



Заключение:
Это объединённый список уникальных задач по Java Collections, в которых учитываются задачи различного уровня сложности (от middle до senior).



