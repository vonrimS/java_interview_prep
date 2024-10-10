# Топ-20 `Java` задач по `Concurrency`

1. Создание и запуск потоков:
Напишите программу, которая создает и запускает 3 отдельных потока с использованием как реализации интерфейса Runnable, так и расширения класса Thread.

2. Синхронизация потоков с использованием ключевого слова synchronized:
Напишите программу, которая создает два потока, увеличивающих общий счетчик, и обеспечьте, чтобы потоки корректно синхронизировались при доступе к счетчику.

java
Copy code
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
3. Пример гонки данных (Race Condition):
Создайте пример программы, где возникает гонка данных, и объясните, как эта проблема может быть решена с использованием синхронизации.

4. Использование ExecutorService для управления потоками:
Напишите программу, которая создает пул потоков с помощью ExecutorService и выполняет несколько задач параллельно.

java
Copy code
ExecutorService executor = Executors.newFixedThreadPool(3);
executor.submit(() -> { /* код задачи */ });
executor.shutdown();
5. Использование CountDownLatch:
Напишите программу, где несколько потоков должны завершить выполнение до того, как основной поток продолжит свою работу, используя CountDownLatch.

java
Copy code
CountDownLatch latch = new CountDownLatch(3);
6. Использование CyclicBarrier:
Создайте программу, где несколько потоков должны синхронизироваться на определенной точке выполнения, прежде чем продолжить работу, используя CyclicBarrier.

java
Copy code
CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("Barrier reached!"));
7. Проблема "Производитель-Потребитель" с использованием BlockingQueue:
Реализуйте решение проблемы "Производитель-Потребитель" с использованием BlockingQueue.

java
Copy code
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
8. Использование Semaphore для ограничения доступа к ресурсу:
Напишите программу, которая использует Semaphore, чтобы ограничить доступ к ресурсу для нескольких потоков.

java
Copy code
Semaphore semaphore = new Semaphore(2); // только два потока могут одновременно получить доступ
9. Использование ReentrantLock:
Реализуйте программу с использованием ReentrantLock для блокировки доступа к ресурсу несколькими потоками.

java
Copy code
ReentrantLock lock = new ReentrantLock();
10. Решение задачи "Читатели-Писатели" с использованием ReadWriteLock:
Напишите программу, где несколько потоков могут читать данные одновременно, но только один поток может писать, используя ReadWriteLock.

java
Copy code
ReadWriteLock lock = new ReentrantReadWriteLock();
11. Использование Future и Callable:
Напишите программу, которая использует Callable для выполнения задачи, которая возвращает результат, и получите результат с помощью Future.

java
Copy code
Callable<Integer> task = () -> { return 123; };
Future<Integer> future = executor.submit(task);
12. Пример использования CompletableFuture:
Создайте программу, которая использует CompletableFuture для асинхронного выполнения задач с последовательной обработкой результатов.

java
Copy code
CompletableFuture.supplyAsync(() -> "Result")
    .thenApply(result -> "Processed " + result)
    .thenAccept(System.out::println);
13. Создание демона потока:
Напишите программу, которая создает поток-демон (daemon thread), и объясните, как он работает.

java
Copy code
Thread daemonThread = new Thread(() -> { /* код задачи */ });
daemonThread.setDaemon(true);
daemonThread.start();
14. Пример с использованием ThreadLocal:
Создайте программу, которая использует ThreadLocal для предоставления каждому потоку своей копии переменной.

java
Copy code
ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);
15. Синхронизация с использованием volatile:
Напишите программу, которая использует volatile для синхронизации доступа к переменной между несколькими потоками.

java
Copy code
private volatile boolean flag = false;
16. Реализация простого Executor сервиса:
Создайте свой собственный простой аналог ExecutorService, который управляет выполнением потоков.

17. Использование Phaser для координации фаз между потоками:
Реализуйте программу, которая использует Phaser для управления выполнением фаз между потоками.

java
Copy code
Phaser phaser = new Phaser(3);
18. Обработка прерывания потоков:
Напишите программу, которая демонстрирует, как обработать прерывание (interrupt) потока.

java
Copy code
if (Thread.currentThread().isInterrupted()) {
    throw new InterruptedException();
}
19. Пример дедлока (Deadlock) и его решение:
Создайте пример программы, в которой возникает дедлок между двумя потоками, и предложите способ избежать дедлока.

20. Пример использования Exchanger:
Создайте программу, которая использует Exchanger для обмена данными между двумя потоками.

java
Copy code
Exchanger<String> exchanger = new Exchanger<>();