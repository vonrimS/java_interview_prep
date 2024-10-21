# Топ `Java` задач по `Collections`

1. **Реализуйте собственную HashMap**

Создайте свою реализацию `HashMap` с использованием открытой адресации или цепочек для разрешения коллизий.

<details>
 <summary>Решение 1</summary> 
 </br>

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

</details>
<br>

2. **Топ-`k` часто встречающихся элементов в списке**

Используйте `PriorityQueue` для нахождения **k** наиболее частых элементов в списке чисел.

<details>
 <summary>Решение 2</summary> 
 </br>

    public static int[] topKFrequent(int[] nums, int k) {
        // Шаг 1: Подсчет частоты каждого элемента с помощью HashMap
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Шаг 2: Использование PriorityQueue (мин-куча) для хранения k самых частых элементов
        PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<>(
            (a, b) -> a.getValue() - b.getValue() // сравнение по частоте
        );

        // Шаг 3: Наполняем кучу и следим, чтобы размер не превышал k
        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll(); // удаляем элемент с наименьшей частотой
            }
        }

        // Шаг 4: Извлекаем k самых частых элементов из кучи
        int[] topK = new int[k];
        int index = 0;
        while (!minHeap.isEmpty()) {
            topK[index++] = minHeap.poll().getKey();
        }

        return topK;
    }

    // main
    public static void main(String[] args) {
        int[] nums = {1, 1, 1, 2, 2, 3, 3, 3, 3, 4, 5, 5, 5, 5};
        int k = 2;
        
        // Вызов метода и вывод результата
        int[] result = topKFrequent(nums, k);
        for (int num : result) {
            System.out.print(num + " ");
        }
    }

</details>
<br>

3. **Реализуйте очередь с приоритетом с использованием `Heap`**

Создайте собственную реализацию очереди с приоритетом `Priority Queue` с использованием кучи.

<details>
 <summary>Решение 3</summary> 
 </br>

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

</details>
<br>

4. **Найдите медиану в потоке данных с использованием `PriorityQueue`**

Используйте две очереди с приоритетом для нахождения медианы в динамически поступающих данных.

<details>
 <summary>Решение 4</summary> 
 </br>

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

</details>
<br>

5. **Реализуйте пропускную таблицу `Skip List`**

Реализуйте структуру данных `Skip List`, используемую для поиска элементов с `O(log n)` сложностью.

<details>
 <summary>Решение 5</summary> 
 </br>

    public class SkipList {
        // Определение класса узла
        class Node {
            int value;
            Node[] forward; // Массив ссылок на узлы следующего уровня

            public Node(int value, int level) {
                this.value = value;
                forward = new Node[level + 1]; // Уровень узла определяет количество ссылок
            }
        }

        private static final int MAX_LEVEL = 16; // Максимальный уровень для узлов
        private final Random random;
        private Node head; // Головной узел
        private int level; // Текущий уровень

        public SkipList() {
            this.level = 0;
            this.head = new Node(-1, MAX_LEVEL); // -1 как значение для головного узла
            this.random = new Random();
        }

        // Генерация уровня для узла
        private int randomLevel() {
            int lvl = 0;
            while (random.nextInt(2) == 1 && lvl < MAX_LEVEL) {
                lvl++;
            }
            return lvl;
        }

        // Поиск элемента
        public boolean search(int target) {
            Node current = head;
            // Проходим с верхнего уровня вниз
            for (int i = level; i >= 0; i--) {
                while (current.forward[i] != null && current.forward[i].value < target) {
                    current = current.forward[i];
                }
            }
            current = current.forward[0]; // Переход на уровень 0

            // Проверяем, найден ли элемент
            return current != null && current.value == target;
        }

        // Вставка элемента
        public void insert(int value) {
            Node current = head;
            Node[] update = new Node[MAX_LEVEL + 1];
            
            // Находим позиции для вставки нового элемента
            for (int i = level; i >= 0; i--) {
                while (current.forward[i] != null && current.forward[i].value < value) {
                    current = current.forward[i];
                }
                update[i] = current;
            }

            // Генерируем уровень для нового элемента
            int newLevel = randomLevel();
            if (newLevel > level) {
                for (int i = level + 1; i <= newLevel; i++) {
                    update[i] = head;
                }
                level = newLevel;
            }

            // Создаем новый узел
            Node newNode = new Node(value, newLevel);
            for (int i = 0; i <= newLevel; i++) {
                newNode.forward[i] = update[i].forward[i];
                update[i].forward[i] = newNode;
            }
        }

        // Удаление элемента
        public void delete(int value) {
            Node current = head;
            Node[] update = new Node[MAX_LEVEL + 1];
            
            // Находим позиции, где нужно обновить ссылки
            for (int i = level; i >= 0; i--) {
                while (current.forward[i] != null && current.forward[i].value < value) {
                    current = current.forward[i];
                }
                update[i] = current;
            }

            current = current.forward[0]; // Переход на уровень 0

            // Если элемент найден, обновляем ссылки
            if (current != null && current.value == value) {
                for (int i = 0; i <= level; i++) {
                    if (update[i].forward[i] != current) {
                        break;
                    }
                    update[i].forward[i] = current.forward[i];
                }

                // Убираем пустые уровни
                while (level > 0 && head.forward[level] == null) {
                    level--;
                }
            }
        }

        // Вывод пропускного списка
        public void display() {
            for (int i = level; i >= 0; i--) {
                Node node = head.forward[i];
                System.out.print("Level " + i + ": ");
                while (node != null) {
                    System.out.print(node.value + " ");
                    node = node.forward[i];
                }
                System.out.println();
            }
        }

        // Пример использования
        public static void main(String[] args) {
            SkipList skipList = new SkipList();

            skipList.insert(3);
            skipList.insert(6);
            skipList.insert(7);
            skipList.insert(9);
            skipList.insert(12);
            skipList.insert(19);
            skipList.insert(17);
            skipList.insert(26);
            skipList.insert(21);
            skipList.insert(25);

            skipList.display();

            System.out.println("\nSearch 19: " + skipList.search(19)); // true
            System.out.println("Search 15: " + skipList.search(15)); // false

            skipList.delete(19);
            System.out.println("\nAfter deleting 19:");
            skipList.display();
        }
    }

</details>
<br>

6. **Слияние `k` отсортированных списков**

Используйте `PriorityQueue` для слияния `k` отсортированных списков в один отсортированный список.

<details>
 <summary>Решение 6</summary> 
 </br>

    public class MergeKSortedLists {

        // Определение структуры для хранения элемента из списка
        static class Element implements Comparable<Element> {
            int value;
            int listIndex;  // Индекс списка, к которому принадлежит элемент
            int elementIndex;  // Индекс элемента внутри списка

            public Element(int value, int listIndex, int elementIndex) {
                this.value = value;
                this.listIndex = listIndex;
                this.elementIndex = elementIndex;
            }

            // Метод для сравнения элементов по значению для сортировки в PriorityQueue
            @Override
            public int compareTo(Element other) {
                return Integer.compare(this.value, other.value);
            }
        }

        public static List<Integer> mergeKSortedLists(List<List<Integer>> lists) {
            PriorityQueue<Element> pq = new PriorityQueue<>();
            List<Integer> result = new ArrayList<>();

            // Инициализируем очередь с первым элементом каждого списка
            for (int i = 0; i < lists.size(); i++) {
                if (!lists.get(i).isEmpty()) {
                    pq.offer(new Element(lists.get(i).get(0), i, 0));
                }
            }

            // Пока очередь не пуста
            while (!pq.isEmpty()) {
                // Извлекаем минимальный элемент
                Element current = pq.poll();
                result.add(current.value);

                // Если в соответствующем списке есть следующий элемент, добавляем его в очередь
                if (current.elementIndex + 1 < lists.get(current.listIndex).size()) {
                    pq.offer(new Element(
                        lists.get(current.listIndex).get(current.elementIndex + 1),
                        current.listIndex,
                        current.elementIndex + 1
                    ));
                }
            }

            return result;
        }

        public static void main(String[] args) {
            // Пример использования
            List<List<Integer>> lists = Arrays.asList(
                Arrays.asList(1, 4, 5),
                Arrays.asList(1, 3, 4),
                Arrays.asList(2, 6)
            );

            List<Integer> result = mergeKSortedLists(lists);
            System.out.println(result);  // Вывод: [1, 1, 2, 3, 4, 4, 5, 6]
        }
    }

</details>
<br>

7. **Реализуйте двусвязный список с поддержкой `LRU`-кэша**

Реализуйте двусвязный список для хранения элементов в порядке доступа и используйте его для реализации `LRU`-кэша с `O(1)` временем доступа.

<details>
 <summary>Решение 7</summary> 
 </br>

    class LRUCache {
        // Класс для узла двусвязного списка
        class Node {
            int key, value;
            Node prev, next;
            
            Node(int key, int value) {
                this.key = key;
                this.value = value;
            }
        }
        
        private final int capacity;
        private final HashMap<Integer, Node> map;
        private Node head, tail;

        public LRUCache(int capacity) {
            this.capacity = capacity;
            this.map = new HashMap<>();
            this.head = new Node(0, 0); // Фиктивный головной узел
            this.tail = new Node(0, 0); // Фиктивный хвостовой узел
            head.next = tail;
            tail.prev = head;
        }

        // Метод для получения значения по ключу
        public int get(int key) {
            if (map.containsKey(key)) {
                Node node = map.get(key);
                // Удаляем элемент из текущей позиции
                remove(node);
                // Вставляем в начало списка, как самый недавно использованный
                insert(node);
                return node.value;
            }
            return -1; // Если ключа нет в кэше
        }

        // Метод для вставки нового элемента
        public void put(int key, int value) {
            if (map.containsKey(key)) {
                // Если элемент уже существует, удаляем его
                remove(map.get(key));
            }
            if (map.size() == capacity) {
                // Если кэш переполнен, удаляем наименее недавно использованный элемент (хвост)
                remove(tail.prev);
            }
            // Вставляем новый элемент в голову
            insert(new Node(key, value));
        }

        // Удаление узла из двусвязного списка
        private void remove(Node node) {
            map.remove(node.key);
            node.prev.next = node.next;
            node.next.prev = node.prev;
        }

        // Вставка узла в начало списка (голова)
        private void insert(Node node) {
            map.put(node.key, node);
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
        }

        // Метод для отладки — отображение содержимого кэша
        public void displayCache() {
            Node current = head.next;
            System.out.print("Cache state: ");
            while (current != tail) {
                System.out.print("[" + current.key + ":" + current.value + "] ");
                current = current.next;
            }
            System.out.println();
        }
        
        public static void main(String[] args) {
            LRUCache lruCache = new LRUCache(3);
            
            lruCache.put(1, 1);
            lruCache.put(2, 2);
            lruCache.put(3, 3);
            lruCache.displayCache(); // Cache state: [3:3] [2:2] [1:1]
            
            lruCache.get(2); // Доступ к элементу 2 делает его самым недавно использованным
            lruCache.displayCache(); // Cache state: [2:2] [3:3] [1:1]
            
            lruCache.put(4, 4); // Вставка нового элемента 4, удаление самого старого (1)
            lruCache.displayCache(); // Cache state: [4:4] [2:2] [3:3]
            
            lruCache.get(1); // Ключа 1 больше нет в кэше
            lruCache.get(3); // Доступ к элементу 3
            lruCache.displayCache(); // Cache state: [3:3] [4:4] [2:2]
        }
    }

</details>
<br>

8. **Реализация `LRU`-кэша (LRU Cache Implementation)**

Реализуйте `LRU`-кэш (Least Recently Used) с ограничением размера, используя `LinkedHashMap` или `Deque`.

<details>
 <summary>Решение 8</summary> 
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

9. **Нахождение пересечения двух списков**

Напишите метод, который принимает два списка и возвращает новый список, содержащий только те элементы, которые встречаются в обоих списках.

<details>
 <summary>Решение 9</summary> 
 </br>

    // Метод для нахождения пересечения двух списков
    public static List<Integer> findIntersection(List<Integer> list1, List<Integer> list2) {
        // Используем HashSet для хранения элементов первого списка
        HashSet<Integer> set = new HashSet<>(list1);
        List<Integer> intersection = new ArrayList<>();

        // Проходим по второму списку и проверяем наличие элементов в первом множестве
        for (Integer element : list2) {
            if (set.contains(element)) {
                intersection.add(element);
                set.remove(element); // Чтобы избежать дубликатов
            }
        }

        return intersection;
    }

</details>
<br>

10. **Удаление дубликатов из `ArrayList`**

Напишите метод, который удаляет дубликаты из `ArrayList<Integer>` без использования `Set`.

<details>
 <summary>Решение 10</summary> 
 </br>

    public List<Integer> removeDuplicates(List<Integer> list) {
        List<Integer> result = new ArrayList<>();
        for (Integer num : list) {
            if (!result.contains(num)) {
                result.add(num);
            }
        }
        return result;
    }

</details>
<br>


11. **Реализуйте метод для проверки баланса скобок с использованием стека**

Реализуйте алгоритм для проверки, сбалансированы ли строки с различными типами скобок.

<details>
 <summary>Решение 11</summary> 
 </br>

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

</details>
<br>

12. **Нахождение всех подмножеств множества (`power set`)**

Напишите функцию для нахождения всех подмножеств (`power set`) множества, используя коллекции.

<details>
 <summary>Решение 12</summary> 
 </br>

    public class PowerSet {

        // Метод для нахождения всех подмножеств множества (Power Set)
        public static List<List<Integer>> findPowerSet(List<Integer> set) {
            List<List<Integer>> powerSet = new ArrayList<>();
            
            // Начинаем с пустого подмножества
            powerSet.add(new ArrayList<>());
            
            // Проходим по каждому элементу исходного множества
            for (Integer element : set) {
                // Создаем временный список для хранения новых подмножеств
                List<List<Integer>> newSubsets = new ArrayList<>();
                
                // Для каждого существующего подмножества добавляем новый элемент
                for (List<Integer> subset : powerSet) {
                    List<Integer> newSubset = new ArrayList<>(subset); // Копируем текущее подмножество
                    newSubset.add(element); // Добавляем новый элемент
                    newSubsets.add(newSubset); // Добавляем новое подмножество в список
                }
                
                // Добавляем все новые подмножества к нашему powerSet
                powerSet.addAll(newSubsets);
            }
            
            return powerSet;
        }

        public static void main(String[] args) {
            // Пример использования
            List<Integer> set = new ArrayList<>();
            set.add(1);
            set.add(2);
            set.add(3);

            List<List<Integer>> powerSet = findPowerSet(set);

            // Вывод всех подмножеств
            System.out.println("Power Set:");
            for (List<Integer> subset : powerSet) {
                System.out.println(subset);
            }
        }
    }

</details>
<br>


13. **Реализуйте красно-черное дерево (`Red-Black Tree`)**

Создайте собственную реализацию красно-черного дерева с поддержкой операций вставки, удаления и балансировки.

<details>
 <summary>Решение 13</summary> 
 </br>

    class RedBlackTree {

        private static final boolean RED = true;
        private static final boolean BLACK = false;

        // Класс узла дерева
        class Node {
            int data;
            Node left, right, parent;
            boolean color;

            Node(int data) {
                this.data = data;
                this.color = RED; // Все новые узлы сначала вставляются как красные
            }
        }

        private Node root;

        // Левый поворот
        private void rotateLeft(Node node) {
            Node rightChild = node.right;
            node.right = rightChild.left;

            if (rightChild.left != null) {
                rightChild.left.parent = node;
            }

            rightChild.parent = node.parent;

            if (node.parent == null) {
                root = rightChild;
            } else if (node == node.parent.left) {
                node.parent.left = rightChild;
            } else {
                node.parent.right = rightChild;
            }

            rightChild.left = node;
            node.parent = rightChild;
        }

        // Правый поворот
        private void rotateRight(Node node) {
            Node leftChild = node.left;
            node.left = leftChild.right;

            if (leftChild.right != null) {
                leftChild.right.parent = node;
            }

            leftChild.parent = node.parent;

            if (node.parent == null) {
                root = leftChild;
            } else if (node == node.parent.right) {
                node.parent.right = leftChild;
            } else {
                node.parent.left = leftChild;
            }

            leftChild.right = node;
            node.parent = leftChild;
        }

        // Вставка узла
        public void insert(int data) {
            Node newNode = new Node(data);
            root = bstInsert(root, newNode);
            fixViolations(newNode);
        }

        // Бинарная вставка (вставляем узел как в обычное двоичное дерево поиска)
        private Node bstInsert(Node root, Node node) {
            if (root == null) {
                return node;
            }

            if (node.data < root.data) {
                root.left = bstInsert(root.left, node);
                root.left.parent = root;
            } else if (node.data > root.data) {
                root.right = bstInsert(root.right, node);
                root.right.parent = root;
            }

            return root;
        }

        // Исправление нарушений после вставки
        private void fixViolations(Node node) {
            Node parent = null;
            Node grandParent = null;

            while (node != root && node.color != BLACK && node.parent.color == RED) {
                parent = node.parent;
                grandParent = parent.parent;

                // Случай A: Родитель является левым ребенком деда
                if (parent == grandParent.left) {
                    Node uncle = grandParent.right;

                    // Случай 1: Дядя является красным (перекрашиваем)
                    if (uncle != null && uncle.color == RED) {
                        grandParent.color = RED;
                        parent.color = BLACK;
                        uncle.color = BLACK;
                        node = grandParent;
                    } else {
                        // Случай 2: Узел является правым ребенком (левый поворот)
                        if (node == parent.right) {
                            rotateLeft(parent);
                            node = parent;
                            parent = node.parent;
                        }

                        // Случай 3: Узел является левым ребенком (правый поворот и перекрашивание)
                        rotateRight(grandParent);
                        boolean tmpColor = parent.color;
                        parent.color = grandParent.color;
                        grandParent.color = tmpColor;
                        node = parent;
                    }
                } else {
                    // Случай B: Родитель является правым ребенком деда
                    Node uncle = grandParent.left;

                    // Случай 1: Дядя является красным (перекрашиваем)
                    if (uncle != null && uncle.color == RED) {
                        grandParent.color = RED;
                        parent.color = BLACK;
                        uncle.color = BLACK;
                        node = grandParent;
                    } else {
                        // Случай 2: Узел является левым ребенком (правый поворот)
                        if (node == parent.left) {
                            rotateRight(parent);
                            node = parent;
                            parent = node.parent;
                        }

                        // Случай 3: Узел является правым ребенком (левый поворот и перекрашивание)
                        rotateLeft(grandParent);
                        boolean tmpColor = parent.color;
                        parent.color = grandParent.color;
                        grandParent.color = tmpColor;
                        node = parent;
                    }
                }
            }

            // Корень всегда черный
            root.color = BLACK;
        }

        // Утилита для печати дерева
        public void inorder() {
            inorderHelper(root);
        }

        // Обход дерева
        private void inorderHelper(Node root) {
            if (root != null) {
                inorderHelper(root.left);
                System.out.print(root.data + " ");
                inorderHelper(root.right);
            }
        }

        // Поиск по дереву
        public Node search(int key) {
            return searchHelper(root, key);
        }

        private Node searchHelper(Node root, int key) {
            if (root == null || root.data == key) {
                return root;
            }

            if (key < root.data) {
                return searchHelper(root.left, key);
            }

            return searchHelper(root.right, key);
        }

        public static void main(String[] args) {
            RedBlackTree tree = new RedBlackTree();

            // Вставляем элементы
            tree.insert(10);
            tree.insert(20);
            tree.insert(30);
            tree.insert(15);

            // Печать дерева
            tree.inorder();
        }
    }

</details>
<br>

14. **Реализуйте алгоритм на основе `Trie` для поиска строк**

Реализуйте структуру данных `Trie` для поиска префиксов в множестве строк.

<details>
 <summary>Решение 14</summary> 
 </br>

    public class Trie {

        // Узел Trie
        class TrieNode {
            // HashMap для хранения дочерних узлов
            Map<Character, TrieNode> children = new HashMap<>();
            // Флаг, указывающий, является ли данный узел концом слова
            boolean isEndOfWord = false;
        }

        private final TrieNode root;

        // Конструктор
        public Trie() {
            root = new TrieNode();
        }

        // Вставка слова в Trie
        public void insert(String word) {
            TrieNode current = root;
            for (char ch : word.toCharArray()) {
                current = current.children.computeIfAbsent(ch, c -> new TrieNode());
            }
            current.isEndOfWord = true; // Отметить конец слова
        }

        // Поиск полного слова в Trie
        public boolean search(String word) {
            TrieNode current = root;
            for (char ch : word.toCharArray()) {
                TrieNode node = current.children.get(ch);
                if (node == null) {
                    return false; // Если символ не найден, слово отсутствует
                }
                current = node;
            }
            return current.isEndOfWord; // Проверяем, является ли конец текущего узла концом слова
        }

        // Поиск префикса в Trie
        public boolean startsWith(String prefix) {
            TrieNode current = root;
            for (char ch : prefix.toCharArray()) {
                TrieNode node = current.children.get(ch);
                if (node == null) {
                    return false; // Префикс не найден
                }
                current = node;
            }
            return true; // Префикс существует
        }

        // Метод для демонстрации работы Trie
        public static void main(String[] args) {
            Trie trie = new Trie();
            
            trie.insert("apple");
            trie.insert("app");
            trie.insert("bat");
            trie.insert("ball");

            // Проверка поиска слов
            System.out.println(trie.search("apple"));  // true
            System.out.println(trie.search("app"));    // true
            System.out.println(trie.search("appl"));   // false
            System.out.println(trie.search("bat"));    // true
            System.out.println(trie.search("ball"));   // true
            System.out.println(trie.search("balls"));  // false

            // Проверка поиска префиксов
            System.out.println(trie.startsWith("app"));  // true
            System.out.println(trie.startsWith("bal"));  // true
            System.out.println(trie.startsWith("ba"));   // true
            System.out.println(trie.startsWith("bat"));  // true
            System.out.println(trie.startsWith("bats")); // false
        }
    }

</details>
<br>


15. **Напишите алгоритм Дейкстры с использованием `PriorityQueue`**

Реализуйте алгоритм поиска кратчайшего пути в графе с использованием `PriorityQueue`.

<details>
 <summary>Решение 15</summary> 
 </br>

    class Dijkstra {

        // Вспомогательный класс для хранения ребер графа
        static class Edge {
            int target;  // целевая вершина
            int weight;  // вес ребра

            Edge(int target, int weight) {
                this.target = target;
                this.weight = weight;
            }
        }

        // Вспомогательный класс для хранения текущего состояния вершины (вершина, текущее расстояние)
        static class Node implements Comparable<Node> {
            int vertex;  // вершина
            int distance;  // текущее расстояние

            Node(int vertex, int distance) {
                this.vertex = vertex;
                this.distance = distance;
            }

            // Сравнение по расстоянию для PriorityQueue
            @Override
            public int compareTo(Node other) {
                return Integer.compare(this.distance, other.distance);
            }
        }

        // Метод для выполнения алгоритма Дейкстры
        public static int[] dijkstra(int start, List<List<Edge>> graph) {
            int n = graph.size();  // количество вершин в графе
            int[] distances = new int[n];  // массив для хранения кратчайших расстояний
            Arrays.fill(distances, Integer.MAX_VALUE);  // Инициализация расстояний как бесконечные

            distances[start] = 0;  // Начальная вершина имеет расстояние 0

            PriorityQueue<Node> priorityQueue = new PriorityQueue<>();
            priorityQueue.add(new Node(start, 0));  // Добавляем начальную вершину в очередь

            while (!priorityQueue.isEmpty()) {
                Node currentNode = priorityQueue.poll();  // Извлекаем вершину с минимальным расстоянием
                int currentVertex = currentNode.vertex;
                int currentDistance = currentNode.distance;

                // Если текущее расстояние больше, чем уже найденное для этой вершины, пропускаем
                if (currentDistance > distances[currentVertex]) {
                    continue;
                }

                // Проходим по всем соседям текущей вершины
                for (Edge edge : graph.get(currentVertex)) {
                    int neighbor = edge.target;
                    int newDistance = currentDistance + edge.weight;

                    // Если найден путь короче, обновляем расстояние до соседа и добавляем его в очередь
                    if (newDistance < distances[neighbor]) {
                        distances[neighbor] = newDistance;
                        priorityQueue.add(new Node(neighbor, newDistance));
                    }
                }
            }

            return distances;  // Возвращаем массив кратчайших расстояний от стартовой вершины
        }

        public static void main(String[] args) {
            // Пример графа
            int n = 5;  // количество вершин

            // Инициализация графа (список смежности)
            List<List<Edge>> graph = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                graph.add(new ArrayList<>());
            }

            // Добавляем ребра
            graph.get(0).add(new Edge(1, 10));
            graph.get(0).add(new Edge(4, 5));
            graph.get(1).add(new Edge(2, 1));
            graph.get(1).add(new Edge(4, 2));
            graph.get(2).add(new Edge(3, 4));
            graph.get(3).add(new Edge(0, 7));
            graph.get(3).add(new Edge(2, 6));
            graph.get(4).add(new Edge(1, 3));
            graph.get(4).add(new Edge(2, 9));
            graph.get(4).add(new Edge(3, 2));

            // Выполняем алгоритм Дейкстры с начальной вершиной 0
            int[] distances = dijkstra(0, graph);

            // Вывод кратчайших расстояний от начальной вершины до каждой вершины
            System.out.println("Кратчайшие расстояния от начальной вершины:");
            for (int i = 0; i < distances.length; i++) {
                System.out.println("Вершина " + i + ": " + distances[i]);
            }
        }
    }

</details>
<br>


16. **Реализуйте топологическую сортировку на графе**

Реализуйте топологическую сортировку направленного ациклического графа (`DAG`) с использованием `Stack`.

<details>
 <summary>Решение 16</summary> 
 </br>

    public class TopologicalSort {

        // Вспомогательный класс для представления графа
        static class Graph {
            private final int vertices;  // Количество вершин
            private final List<List<Integer>> adj;  // Список смежности для графа

            // Конструктор графа
            public Graph(int vertices) {
                this.vertices = vertices;
                adj = new ArrayList<>(vertices);
                for (int i = 0; i < vertices; i++) {
                    adj.add(new ArrayList<>());
                }
            }

            // Добавление ребра в граф
            public void addEdge(int from, int to) {
                adj.get(from).add(to);
            }

            // Топологическая сортировка
            public void topologicalSort() {
                Stack<Integer> stack = new Stack<>();
                boolean[] visited = new boolean[vertices];

                // Вызываем рекурсивную функцию для каждого непосещенного узла
                for (int i = 0; i < vertices; i++) {
                    if (!visited[i]) {
                        topologicalSortUtil(i, visited, stack);
                    }
                }

                // Распаковываем стек, чтобы вывести вершины в порядке топологической сортировки
                System.out.println("Топологическая сортировка графа:");
                while (!stack.isEmpty()) {
                    System.out.print(stack.pop() + " ");
                }
            }

            // Вспомогательная рекурсивная функция для топологической сортировки
            private void topologicalSortUtil(int vertex, boolean[] visited, Stack<Integer> stack) {
                // Помечаем текущую вершину как посещенную
                visited[vertex] = true;

                // Проходим по всем соседям текущей вершины
                for (Integer neighbor : adj.get(vertex)) {
                    if (!visited[neighbor]) {
                        topologicalSortUtil(neighbor, visited, stack);
                    }
                }

                // После посещения всех соседей добавляем вершину в стек
                stack.push(vertex);
            }
        }

        public static void main(String[] args) {
            // Пример графа с 6 вершинами (0, 1, 2, 3, 4, 5)
            Graph graph = new Graph(6);

            // Добавляем ребра в граф
            graph.addEdge(5, 2);
            graph.addEdge(5, 0);
            graph.addEdge(4, 0);
            graph.addEdge(4, 1);
            graph.addEdge(2, 3);
            graph.addEdge(3, 1);

            // Выполняем топологическую сортировку
            graph.topologicalSort();
        }
    }

</details>
<br>

17. **Реализуйте метод для детекции цикла в связанном списке**

Реализуйте алгоритм для проверки наличия цикла в односвязном списке с использованием `двух указателей` - алгоритм Флойда.

<details>
 <summary>Решение 17</summary> 
 </br>

    public class LinkedListCycleDetection {

        // Класс для представления узла односвязного списка
        static class ListNode {
            int value;
            ListNode next;

            ListNode(int value) {
                this.value = value;
                this.next = null;
            }
        }

        // Метод для проверки наличия цикла в списке
        public static boolean hasCycle(ListNode head) {
            if (head == null || head.next == null) {
                return false; // Если список пуст или состоит из одного элемента, цикла нет
            }

            ListNode slow = head;  // Медленный указатель (черепаха)
            ListNode fast = head;  // Быстрый указатель (заяц)

            // Пока быстрый указатель и его следующий узел не равны null
            while (fast != null && fast.next != null) {
                slow = slow.next;         // Медленный указатель перемещается на один шаг
                fast = fast.next.next;    // Быстрый указатель перемещается на два шага

                // Если быстрый указатель догнал медленный, значит, есть цикл
                if (slow == fast) {
                    return true;
                }
            }

            return false; // Если дошли до конца списка, цикла нет
        }

        public static void main(String[] args) {
            // Пример без цикла
            ListNode head = new ListNode(1);
            head.next = new ListNode(2);
            head.next.next = new ListNode(3);
            head.next.next.next = new ListNode(4);

            System.out.println("Цикл: " + hasCycle(head)); // Вывод: Цикл: false

            // Пример с циклом
            head.next.next.next.next = head.next; // Создаем цикл

            System.out.println("Цикл: " + hasCycle(head)); // Вывод: Цикл: true
        }
    }

</details>
<br>

18. **Перевод `Map` в `List` и обратно**

Напишите метод, который преобразует `Map<Integer, String>` в `List<Integer>` и обратно.

<details>
 <summary>Решение 18</summary> 
 </br>

    public class MapListConverter {

        // Метод для преобразования Map<Integer, String> в List<Integer>
        public static List<Integer> mapToList(Map<Integer, String> map) {
            // Извлекаем все ключи из карты и возвращаем их как список
            return new ArrayList<>(map.keySet());
        }

        // Метод для преобразования List<Integer> и List<String> обратно в Map<Integer, String>
        public static Map<Integer, String> listToMap(List<Integer> keys, List<String> values) {
            if (keys.size() != values.size()) {
                throw new IllegalArgumentException("Размеры списков ключей и значений должны совпадать.");
            }

            Map<Integer, String> map = new HashMap<>();
            for (int i = 0; i < keys.size(); i++) {
                map.put(keys.get(i), values.get(i));
            }
            return map;
        }

        public static void main(String[] args) {
            // Пример Map<Integer, String>
            Map<Integer, String> map = new HashMap<>();
            map.put(1, "One");
            map.put(2, "Two");
            map.put(3, "Three");

            // Преобразуем Map в List
            List<Integer> keyList = mapToList(map);
            System.out.println("Список ключей: " + keyList);

            // Пример списка значений
            List<String> valueList = new ArrayList<>(Arrays.asList("One", "Two", "Three"));

            // Преобразуем List обратно в Map
            Map<Integer, String> newMap = listToMap(keyList, valueList);
            System.out.println("Восстановленный Map: " + newMap);
        }
    }

</details>
<br>


19. **Удаление элементов из `List` по условию**

Напишите метод, который удаляет все элементы из `ArrayList<String>`, которые содержат определённую подстроку.

<details>
 <summary>Решение 19</summary> 
 </br>
    
    public class RemoveBySubstring {

        // Метод для удаления элементов, содержащих определенную подстроку
        public static void removeElementsContainingSubstring(List<String> list, String substring) {
            // Используем метод removeIf для удаления элементов, которые содержат подстроку
            list.removeIf(element -> element.contains(substring));
        }

        public static void main(String[] args) {
            // Пример ArrayList<String>
            List<String> list = new ArrayList<>();
            list.add("apple");
            list.add("banana");
            list.add("cherry");
            list.add("apricot");
            list.add("grape");

            // Подстрока для удаления
            String substring = "ap";

            System.out.println("Список до удаления: " + list);

            // Удаляем элементы, содержащие подстроку "ap"
            removeElementsContainingSubstring(list, substring);

            System.out.println("Список после удаления: " + list);
        }
    }

</details>
<br>

20. **Проверка, содержится ли один список в другом**

Напишите метод, который проверяет, содержится ли один список как подсписок внутри другого списка.

<details>
 <summary>Решение 20</summary> 
 </br>

    public class SublistChecker {

        // Метод для проверки, содержится ли подсписок sublist в списке list
        public static boolean containsSublist(List<Integer> list, List<Integer> sublist) {
            // Если подсписок пустой, он всегда содержится в списке
            if (sublist.isEmpty()) {
                return true;
            }

            // Если основной список пустой или подсписок больше исходного списка, возвращаем false
            if (list.isEmpty() || sublist.size() > list.size()) {
                return false;
            }

            // Проходим по списку и проверяем, начинается ли подсписок с текущей позиции
            for (int i = 0; i <= list.size() - sublist.size(); i++) {
                // Проверяем, совпадают ли все элементы подсписка
                boolean isMatch = true;
                for (int j = 0; j < sublist.size(); j++) {
                    if (!list.get(i + j).equals(sublist.get(j))) {
                        isMatch = false;
                        break;
                    }
                }

                // Если подсписок найден, возвращаем true
                if (isMatch) {
                    return true;
                }
            }

            // Если подсписок не найден, возвращаем false
            return false;
        }

        public static void main(String[] args) {
            // Пример списков
            List<Integer> list = List.of(1, 2, 3, 4, 5, 6);
            List<Integer> sublist = List.of(3, 4, 5);

            // Проверка, содержится ли подсписок в основном списке
            boolean result = containsSublist(list, sublist);
            System.out.println("Подсписок содержится: " + result);  // Вывод: Подсписок содержится: true

            // Другой пример, когда подсписок отсутствует
            List<Integer> sublist2 = List.of(4, 5, 7);
            boolean result2 = containsSublist(list, sublist2);
            System.out.println("Подсписок содержится: " + result2);  // Вывод: Подсписок содержится: false
        }
    }

</details>
<br>


21. **Проверка уникальности элементов в массиве**

Напишите метод, который проверяет, содержатся ли все элементы массива уникальными, используя только `List` и `Set`.

<details>
 <summary>Решение 21</summary> 
 </br>

    public class UniqueElementsChecker {

        // Метод для проверки уникальности элементов в массиве
        public static boolean areElementsUnique(List<Integer> list) {
            // Создаем Set для хранения уникальных элементов
            Set<Integer> uniqueElements = new HashSet<>();

            // Проходим по всем элементам списка
            for (Integer element : list) {
                // Если элемент уже есть в Set, то элементы не уникальны
                if (!uniqueElements.add(element)) {
                    return false;
                }
            }

            // Если все элементы были добавлены, они уникальны
            return true;
        }

        public static void main(String[] args) {
            // Пример массива (списка)
            List<Integer> list1 = List.of(1, 2, 3, 4, 5);
            List<Integer> list2 = List.of(1, 2, 3, 3, 4);

            // Проверяем, уникальны ли элементы
            System.out.println("Уникальные элементы в list1: " + areElementsUnique(list1)); // true
            System.out.println("Уникальные элементы в list2: " + areElementsUnique(list2)); // false
        }
    }

</details>
<br>


22. **Напишите функцию для сортировки большого файла с числами**

Создайте алгоритм для сортировки файла, который не помещается в память, используя `External Sort` и структуры данных для работы с потоками.

<details>
 <summary>Решение 22</summary> 
 </br>

    public class ExternalSort {

        // Константа для определения максимально возможного размера части файла, который можно загрузить в память
        private static final int MAX_CHUNK_SIZE = 100000; // 100k чисел, это можно адаптировать под доступную память

        // Метод для сортировки большого файла
        public static void externalSort(String inputFile, String outputFile) throws IOException {
            // Список для хранения временных файлов
            List<File> sortedChunks = new ArrayList<>();

            // Разбиваем входной файл на отсортированные части (chunks)
            try (BufferedReader reader = new BufferedReader(new FileReader(inputFile))) {
                List<Integer> buffer = new ArrayList<>(MAX_CHUNK_SIZE);
                String line;

                // Читаем файл порциями, каждая порция сортируется и сохраняется во временный файл
                while ((line = reader.readLine()) != null) {
                    buffer.add(Integer.parseInt(line));

                    if (buffer.size() >= MAX_CHUNK_SIZE) {
                        sortedChunks.add(sortAndSaveChunk(buffer));
                        buffer.clear(); // Очищаем буфер для следующей порции
                    }
                }

                // Сортируем и сохраняем последнюю часть, если остались данные
                if (!buffer.isEmpty()) {
                    sortedChunks.add(sortAndSaveChunk(buffer));
                }
            }

            // Сливаем все отсортированные части (chunks) в один файл
            mergeSortedChunks(sortedChunks, outputFile);

            // Удаляем временные файлы
            for (File file : sortedChunks) {
                file.delete();
            }
        }

        // Метод для сортировки порции данных и сохранения во временный файл
        private static File sortAndSaveChunk(List<Integer> chunk) throws IOException {
            // Сортируем порцию данных
            Collections.sort(chunk);

            // Создаем временный файл для сохранения отсортированной порции
            File tempFile = File.createTempFile("sortedChunk", ".txt");
            tempFile.deleteOnExit();

            // Записываем отсортированную порцию в файл
            try (BufferedWriter writer = new BufferedWriter(new FileWriter(tempFile))) {
                for (Integer num : chunk) {
                    writer.write(num.toString());
                    writer.newLine();
                }
            }

            return tempFile;
        }
    }

</details>
<br>

23. **Реализуйте механизм автоматического удаления "устаревших" элементов в коллекции**

Используйте `ConcurrentHashMap` с `ScheduledExecutorService`, чтобы автоматически удалять элементы через определенное время после их добавления.

<details>
 <summary>Решение 23</summary> 
 </br>

    public class ExpiringMap<K, V> {

        private final ConcurrentHashMap<K, V> map;
        private final ScheduledExecutorService scheduler;

        // Конструктор
        public ExpiringMap() {
            this.map = new ConcurrentHashMap<>();
            this.scheduler = Executors.newScheduledThreadPool(1);  // Один поток для планирования задач
        }

        // Метод для добавления элемента с временем жизни
        public void put(K key, V value, long duration, TimeUnit timeUnit) {
            // Добавляем элемент в карту
            map.put(key, value);

            // Планируем удаление элемента через указанное время
            scheduler.schedule(() -> {
                // Удаляем элемент после истечения времени
                map.remove(key);
                System.out.println("Элемент с ключом " + key + " удалён по истечению времени");
            }, duration, timeUnit);
        }

        // Метод для получения элемента
        public V get(K key) {
            return map.get(key);
        }

        // Метод для удаления элемента вручную
        public void remove(K key) {
            map.remove(key);
        }

        // Метод для получения всех элементов (только для демонстрации)
        public Set<K> keySet() {
            return map.keySet();
        }

        // Метод для остановки планировщика
        public void shutdown() {
            scheduler.shutdown();
        }

        public static void main(String[] args) throws InterruptedException {
            // Создаем объект ExpiringMap
            ExpiringMap<String, String> expiringMap = new ExpiringMap<>();

            // Добавляем элементы с разным временем жизни
            expiringMap.put("key1", "value1", 5, TimeUnit.SECONDS);
            expiringMap.put("key2", "value2", 10, TimeUnit.SECONDS);
            expiringMap.put("key3", "value3", 15, TimeUnit.SECONDS);

            // Периодически выводим текущее состояние карты
            for (int i = 0; i < 20; i++) {
                System.out.println("Текущие ключи: " + expiringMap.keySet());
                Thread.sleep(3000);  // Ожидание 3 секунды между выводами
            }

            // Останавливаем планировщик
            expiringMap.shutdown();
        }
    }

</details>
<br>


24. **Напишите реализацию бин-поиска в отсортированном `ArrayList`**

Реализуйте бинарный поиск в `ArrayList` с соблюдением инвариантов и логики работы поиска в списках.

<details>
 <summary>Решение 24</summary> 
 </br>

    public class BinarySearch {

        // Метод бинарного поиска в отсортированном ArrayList
        public static int binarySearch(List<Integer> list, int target) {
            int left = 0;
            int right = list.size() - 1;

            while (left <= right) {
                int mid = left + (right - left) / 2;  // Для предотвращения переполнения

                // Проверяем, равен ли элемент в середине искомому
                if (list.get(mid) == target) {
                    return mid;  // Возвращаем индекс найденного элемента
                }

                // Если элемент в середине больше искомого, ищем в левой половине
                if (list.get(mid) > target) {
                    right = mid - 1;
                } else {
                    // Если элемент в середине меньше искомого, ищем в правой половине
                    left = mid + 1;
                }
            }

            // Если элемент не найден, возвращаем отрицательное значение
            return -(left + 1);
        }

        public static void main(String[] args) {
            // Пример отсортированного списка
            List<Integer> list = new ArrayList<>();
            list.add(1);
            list.add(3);
            list.add(5);
            list.add(7);
            list.add(9);
            list.add(11);

            // Пример поиска элементов
            System.out.println("Индекс элемента 7: " + binarySearch(list, 7));  // Ожидается: 3
            System.out.println("Индекс элемента 4: " + binarySearch(list, 4));  // Ожидается: отрицательное число
            System.out.println("Индекс элемента 11: " + binarySearch(list, 11));  // Ожидается: 5
        }
    }

</details>
<br>


25. **Реализуйте собственный `ConcurrentHashMap`**

Создайте упрощенную версию `ConcurrentHashMap`, которая поддерживает блокировку сегментов (`segmented locks`) для повышения производительности.

<details>
 <summary>Решение 25</summary> 
 </br>

    public class MyConcurrentHashMap<K, V> {

        // Внутренний класс для хранения сегментов
        private static class Segment<K, V> {
            private final Lock lock = new ReentrantLock();  // Блокировка для каждого сегмента
            private final List<Entry<K, V>> bucket = new ArrayList<>();  // Простой список для хранения пар ключ-значение

            // Метод для вставки нового элемента
            public void put(K key, V value) {
                lock.lock();
                try {
                    // Проверяем, существует ли ключ, и обновляем значение, если да
                    for (Entry<K, V> entry : bucket) {
                        if (entry.getKey().equals(key)) {
                            entry.setValue(value);
                            return;
                        }
                    }
                    // Если ключ не найден, добавляем новую пару ключ-значение
                    bucket.add(new Entry<>(key, value));
                } finally {
                    lock.unlock();
                }
            }

            // Метод для получения значения по ключу
            public V get(K key) {
                lock.lock();
                try {
                    for (Entry<K, V> entry : bucket) {
                        if (entry.getKey().equals(key)) {
                            return entry.getValue();
                        }
                    }
                    return null;  // Если ключ не найден, возвращаем null
                } finally {
                    lock.unlock();
                }
            }

            // Метод для удаления элемента по ключу
            public void remove(K key) {
                lock.lock();
                try {
                    bucket.removeIf(entry -> entry.getKey().equals(key));
                } finally {
                    lock.unlock();
                }
            }
        }

        // Класс для хранения пар ключ-значение
        private static class Entry<K, V> {
            private final K key;
            private V value;

            public Entry(K key, V value) {
                this.key = key;
                this.value = value;
            }

            public K getKey() {
                return key;
            }

            public V getValue() {
                return value;
            }

            public void setValue(V value) {
                this.value = value;
            }
        }

        private final Segment<K, V>[] segments;  // Массив сегментов
        private final int segmentCount;

        // Конструктор, который инициализирует сегменты
        public MyConcurrentHashMap(int segmentCount) {
            this.segmentCount = segmentCount;
            this.segments = new Segment[segmentCount];

            for (int i = 0; i < segmentCount; i++) {
                segments[i] = new Segment<>();
            }
        }

        // Метод для вычисления сегмента на основе хеш-кода ключа
        private int getSegmentIndex(K key) {
            int hash = key.hashCode();
            return (hash & 0x7FFFFFFF) % segmentCount;  // Используем положительный хеш и берем модуль по количеству сегментов
        }

        // Вставка нового элемента
        public void put(K key, V value) {
            int segmentIndex = getSegmentIndex(key);
            segments[segmentIndex].put(key, value);
        }

        // Получение элемента по ключу
        public V get(K key) {
            int segmentIndex = getSegmentIndex(key);
            return segments[segmentIndex].get(key);
        }

        // Удаление элемента по ключу
        public void remove(K key) {
            int segmentIndex = getSegmentIndex(key);
            segments[segmentIndex].remove(key);
        }

        public static void main(String[] args) {
            // Пример использования
            MyConcurrentHashMap<String, Integer> map = new MyConcurrentHashMap<>(4);

            map.put("apple", 1);
            map.put("banana", 2);
            map.put("cherry", 3);

            System.out.println("Значение для 'apple': " + map.get("apple"));  // Ожидается: 1
            System.out.println("Значение для 'banana': " + map.get("banana"));  // Ожидается: 2
            System.out.println("Значение для 'cherry': " + map.get("cherry"));  // Ожидается: 3

            map.remove("banana");
            System.out.println("Значение для 'banana' после удаления: " + map.get("banana"));  // Ожидается: null
        }
    }

</details>
<br>


26. **Напишите функцию для проверки, является ли граф двусвязным, используя `BFS/DFS`**

Используйте граф, реализованный на основе списков смежности, и примените `BFS` или `DFS` для проверки, является ли граф двусвязным.

<details>
 <summary>Решение 26</summary> 
 </br>

    public class BiconnectedGraph {

        private final int vertices; // Количество вершин
        private final List<List<Integer>> adjList; // Список смежности графа
        private int time = 0; // Переменная для отслеживания времени посещения узлов

        // Конструктор графа
        public BiconnectedGraph(int vertices) {
            this.vertices = vertices;
            adjList = new ArrayList<>(vertices);
            for (int i = 0; i < vertices; i++) {
                adjList.add(new ArrayList<>());
            }
        }

        // Метод для добавления ребра в граф
        public void addEdge(int u, int v) {
            adjList.get(u).add(v);
            adjList.get(v).add(u); // Граф неориентированный, поэтому добавляем ребро в обе стороны
        }

        // Метод для проверки, является ли граф двусвязным
        public boolean isBiconnected() {
            boolean[] visited = new boolean[vertices]; // Отслеживаем, были ли посещены вершины
            int[] discoveryTime = new int[vertices]; // Время посещения для каждой вершины
            int[] low = new int[vertices]; // Минимальное достижимое время для каждой вершины
            int[] parent = new int[vertices]; // Массив для отслеживания родителей в DFS
            Arrays.fill(parent, -1); // Инициализируем массив родителей значениями -1

            // Переменная для отслеживания, является ли граф связным и не содержит ли артикуляционные точки
            boolean[] isArticulationPoint = new boolean[vertices];

            // Запускаем DFS с нулевой вершины
            if (!dfs(0, visited, discoveryTime, low, parent, isArticulationPoint)) {
                return false; // Граф не связен или содержит артикуляционные точки
            }

            // Проверяем, что все вершины были посещены (граф связен)
            for (boolean v : visited) {
                if (!v) {
                    return false; // Граф не связен
                }
            }

            // Если ни одна вершина не является артикуляционной, граф двусвязный
            return true;
        }

        // Вспомогательный метод для выполнения DFS
        private boolean dfs(int u, boolean[] visited, int[] discoveryTime, int[] low, int[] parent, boolean[] isArticulationPoint) {
            int children = 0; // Число дочерних вершин для текущей вершины
            visited[u] = true; // Помечаем вершину как посещённую
            discoveryTime[u] = low[u] = ++time; // Устанавливаем время посещения и низкое время

            // Проходим по всем соседям текущей вершины
            for (int v : adjList.get(u)) {
                if (!visited[v]) { // Если соседняя вершина не была посещена
                    children++;
                    parent[v] = u; // Устанавливаем родительскую вершину для v
                    if (!dfs(v, visited, discoveryTime, low, parent, isArticulationPoint)) {
                        return false;
                    }

                    // Обновляем низкое время текущей вершины
                    low[u] = Math.min(low[u], low[v]);

                    // Если u — корень DFS и имеет более одного ребёнка, она артикуляционная
                    if (parent[u] == -1 && children > 1) {
                        isArticulationPoint[u] = true;
                    }

                    // Если u не корень и низкое время v больше или равно времени посещения u, u артикуляционная
                    if (parent[u] != -1 && low[v] >= discoveryTime[u]) {
                        isArticulationPoint[u] = true;
                    }

                } else if (v != parent[u]) {
                    // Обновляем низкое время для u, если v уже была посещена и не является родительской вершиной
                    low[u] = Math.min(low[u], discoveryTime[v]);
                }
            }

            // Если находим артикуляционную точку, граф не двусвязный
            if (isArticulationPoint[u]) {
                return false;
            }

            return true;
        }

        public static void main(String[] args) {
            BiconnectedGraph graph = new BiconnectedGraph(5);

            // Пример графа
            graph.addEdge(0, 1);
            graph.addEdge(1, 2);
            graph.addEdge(2, 3);
            graph.addEdge(3, 4);
            graph.addEdge(4, 0);

            if (graph.isBiconnected()) {
                System.out.println("Граф является двусвязным.");
            } else {
                System.out.println("Граф НЕ является двусвязным.");
            }
        }
    }

</details>
<br>

27. **Напишите реализацию слияния интервалов (`merge intervals`)**

Дана коллекция интервалов, объедините все пересекающиеся интервалы и верните результат в виде списка непересекающихся интервалов.

<details>
 <summary>Решение 27</summary> 
 </br>

    public class MergeIntervals {

        // Вспомогательный класс для хранения интервалов
        static class Interval {
            int start;
            int end;

            Interval(int start, int end) {
                this.start = start;
                this.end = end;
            }

            @Override
            public String toString() {
                return "[" + start + ", " + end + "]";
            }
        }

        // Метод для слияния пересекающихся интервалов
        public static List<Interval> merge(List<Interval> intervals) {
            // Если список пуст или содержит один интервал, возвращаем его как есть
            if (intervals.size() <= 1) {
                return intervals;
            }

            // Сортируем интервалы по началу интервала
            intervals.sort(Comparator.comparingInt(i -> i.start));

            // Список для хранения результата
            List<Interval> merged = new ArrayList<>();

            // Инициализируем текущий интервал как первый в списке
            Interval current = intervals.get(0);

            for (int i = 1; i < intervals.size(); i++) {
                Interval next = intervals.get(i);

                // Проверяем, пересекается ли текущий интервал с следующим
                if (current.end >= next.start) {
                    // Объединяем интервалы, если они пересекаются
                    current.end = Math.max(current.end, next.end);
                } else {
                    // Если интервалы не пересекаются, добавляем текущий интервал в результат
                    merged.add(current);
                    current = next; // Обновляем текущий интервал
                }
            }

            // Добавляем последний интервал в результат
            merged.add(current);

            return merged;
        }

        public static void main(String[] args) {
            // Пример ввода интервалов
            List<Interval> intervals = new ArrayList<>(Arrays.asList(
                new Interval(1, 3),
                new Interval(2, 6),
                new Interval(8, 10),
                new Interval(15, 18)
            ));

            // Вызываем метод для слияния интервалов
            List<Interval> result = merge(intervals);

            // Выводим результат
            System.out.println("Объединённые интервалы: " + result);
        }
    }

</details>
<br>


