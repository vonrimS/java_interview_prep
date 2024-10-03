# `Classic Computer Science Problems in Java` (by David Copez)

[назад](../README.md)

* [Fibonacci](#fibonacci)
* [Простейшее сжатие](#простейшее-сжатие)
* [Невскрываемое шифрование](#невскрываемое-шифрование)
* [Вычисление числа Pi](#вычисление-числа-pi)
* [Ханойские башни](#ханойские-башни)


## Fibonacci

```
Ряд Фибоначчи — это такая последовательность чисел, в которой любое число, кроме первого и второго, является суммой двух предыдущих:

0, 1, 1, 2, 3, 5, 8, 13, 21...

Первое число в последовательности Фибоначчи — 0, четвертое — 2. Отсюда следует, что для получения значения любого числа n в последовательности Фибоначчи можно использовать формулу:

fib(n) = fib(n — 1) + fib(n — 2)
```

```java
// это решение выбросит `StackOverflow`
private static int fib(int n) {
    return fib(n - 1) + fib(n - 2);
}
```

### Рекурсивно
```java
private static int fib(int n) {
    if (n < 2)
        return n;
    
    return fib(n - 1) + fib(n - 2);
}
```

### Рекурсивно с распечаткой
```java
    public static void main(String[] args) {
        printFibonacci(10);
    }

    private static void printFibonacci(int count){
       int a = 0, b = 1, c = 1;

       for (int i = 0; i <= count; i++){
           System.out.print(a + ", ");
           a = b;
           b = c;
           c = a + b;
       }
    }
```

### C мемоизацией
```java
static Map<Integer, Integer> memo = new HashMap<>(Map.of(0, 0, 1, 1));

private static int fib(int n) {
    if (!memo.containsKey(n))
            memo.put(n, fib(n - 1) + fib(n - 2));
        
    return memo.get(n);
}
```
### Итеративно
```java
private static int fib(int n) {
        int last = 0, next = 1;
        for (int i = 0; i < n; i++){
            int temp = last;
            last = next;
            next = temp + next;
        }
        
        return last;
    }
```

### Через поток (с выводом на печать всех элементов)
```java
public class Main {
    
    private int last = 0, next = 1;

    public static void main(String[] args) {
        Main main = new Main();
        main.stream().limit(41).forEachOrdered(System.out::println);
    }

    public IntStream stream() {
        return IntStream.generate(() -> {
            int oldLast = last;
            last = next;
            next = oldLast + next;
            return oldLast;
        });
    }
}
```

[наверх](#java-problems)

## Простейшее сжатие

**Сжатие** — это процесс получения данных и их кодирования (изменения формы) таким образом, чтобы они занимали меньше места. 

**Распаковка** предусматривает обратный процесс — возвращение данных в исходную форму.

```java

public class CompressedGene {
    private BitSet bitSet;
    private int length;

    public CompressedGene(String gene) {
        compress(gene);
    }

    private void compress(String gene) {
        length = gene.length();

        // резервирование пространства для всех битов
        bitSet = new BitSet(length * 2);

        // преобразование в верхний регистр для единообразия
        final String upperGene = gene.toUpperCase();

        // преобразование строки нуклеотидов в биты
        for (int i = 0; i < length; i++) {
            final int firstLocation = 2 * i;
            final int secondLocation = 2 * i + 1;
            switch (upperGene.charAt(i)) {
                case 'A': // 00 — следующие два бита
                    bitSet.set(firstLocation, false);
                    bitSet.set(secondLocation, false);
                    break;
                case 'C': // 01 — следующие два бита
                    bitSet.set(firstLocation, false);
                    bitSet.set(secondLocation, true);
                    break;
                case 'G': // 10 — следующие два бита
                    bitSet.set(firstLocation, true);
                    bitSet.set(secondLocation, false);
                    break;
                case 'T': // 11 — следующие два бита
                    bitSet.set(firstLocation, true);
                    bitSet.set(secondLocation, true);
                    break;
                default:
                    throw new IllegalArgumentException("The provided gene String contains characters other than ACGT");
            }
        }
    }

    public String decompress() {
        if (bitSet == null) return "";

        // создание изменяемой позиции для символов в правильном порядке
        StringBuilder builder = new StringBuilder(length);
        for (int i = 0; i < (length * 2); i += 2) {
            final int firstBit = (bitSet.get(i) ? 1 : 0);
            final int secondBit = (bitSet.get(i + 1) ? 1 : 0);
            final int lastBits = firstBit << 1 | secondBit;
            switch (lastBits) {
                case 0b00: // 00 = 'A'
                    builder.append('A');
                    break;
                case 0b01: // 01 = 'C'
                    builder.append('C');
                    break;
                case 0b10: // 10 = 'G'
                    builder.append('G');
                    break;
                case 0b11: // 11 = 'T'
                    builder.append('T');
                    break;
            }
        }
        return builder.toString();
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        final String original =
                "TAGGGATTAACCGTTATATATATATAGCCATGGATCGATTATATAGGGATTAACCGTTATATATATATAGCCATGGATCGATTATA";
        CompressedGene compressed = new CompressedGene(original);
        final String decompressed = compressed.decompress();
        System.out.println(decompressed);
        System.out.println("original is the same as decompressed: " +
                original.equalsIgnoreCase(decompressed));
    }
}
```

[наверх](#java-problems)

## Невскрываемое шифрование

```java
public final class KeyPair {
    public final byte[] key1;
    public final byte[] key2;

    public KeyPair(byte[] key1, byte[] key2) {
        this.key1 = key1;
        this.key2 = key2;
    }
}
```
```java
public class UnbreakableEncryption {

    private static byte[] randomKey(int length){
        byte[] dummy = new byte[length];
        Random random = new Random();
        random.nextBytes(dummy);
        return dummy;
    }

    public static KeyPair encrypt(String original) {
        byte[] originalBytes = original.getBytes();
        byte[] dummyKey = randomKey(originalBytes.length);
        byte[] encryptedKey = new byte[originalBytes.length];
        for (int i = 0; i < originalBytes.length; i++) {
            // Побитовая операция XOR
            encryptedKey[i] = (byte) (originalBytes[i] ^ dummyKey[i]);
        }
        return new KeyPair(dummyKey, encryptedKey);
    }

    public static String decrypt(KeyPair keyPair){
        byte[] decrypted = new byte[keyPair.key1.length];
        for (int i = 0; i < keyPair.key1.length; i++){
            decrypted[i] = (byte) (keyPair.key1[i] ^ keyPair.key2[i]);
        }

        return new String(decrypted);
    }
}
```
```java
public class Main {

    public static void main(String[] args) {
        KeyPair keyPair = encrypt("One");
        String res = decrypt(keyPair);
        System.out.println(res);
    }
}
```

[наверх](#java-problems)

## Вычисление числа Pi

Одна из самых простых — формула Лейбница. Согласно этой формуле следующий бесконечный ряд сходится к числу π:
```
π = 4/1 – 4/3 + 4/5 – 4/7 + 4/9 – 4/11...
```
### Решение:
```java
public class PiCalculator {

    public static double calculatePi(int nTerms) {
        final double numerator = 4.0;
        double denominator = 1.0;
        double operation = 1.0;
        double pi = 0.0;

        for (int i = 0; i < nTerms; i++) {
            pi += operation * (numerator / denominator);
            denominator += 2.0;
            operation *= -1.0;
        }
        return pi;
    }

    public static void main(String[] args) {
        System.out.println(calculatePi(1_000_000));
    }
}
```

[наверх](#java-problems)

## Ханойские башни

```java
public class Hanoi {
    private final int numDiscs;
    public final Stack<Integer> towerA = new Stack<>();
    public final Stack<Integer> towerB = new Stack<>();
    public final Stack<Integer> towerC = new Stack<>();

    public Hanoi(int numDiscs) {
        this.numDiscs = numDiscs;
        for (int i = 1; i <= numDiscs; i++){
                towerA.push(i);
        }
    }

    private void move(Stack<Integer> begin, Stack<Integer> end, Stack<Integer> temp, int n){
        if (n == 1){
            end.push(begin.pop());
        } else {
            move(begin, temp, end, n - 1);
            move(begin, end, temp, 1);
            move(temp, end, begin, n - 1);
        }
    }

    public void solve(){
        move(towerA, towerB, towerC, numDiscs);
    }

}
```

```java
public class Main {

    public static void main(String[] args) {
        Hanoi hanoi = new Hanoi(3);
        System.out.println(hanoi.towerA);
        System.out.println(hanoi.towerB);
        System.out.println(hanoi.towerC);
        hanoi.solve();
        System.out.println("------after--------");
        System.out.println(hanoi.towerA);
        System.out.println(hanoi.towerB);
        System.out.println(hanoi.towerC);
    }
}
```

[наверх](#java-problems) | [назад](../README.md) 