# Design Patterns

* [Creational](#creational)
    * [Singleton](#singleton)
    * [Builder](#builder)
    * [Dependency Injection](#dependency-injection)

# Creational

## Singleton

```java
public class Singleton {
    // Приватное статическое поле для хранения единственного экземпляра
    private static volatile Singleton instance;

    // Приватный конструктор, чтобы предотвратить создание экземпляров извне
    private Singleton() {
        // Конструктор может содержать инициализацию ресурсов
    }

    // Публичный статический метод для получения единственного экземпляра
    public static Singleton getInstance() {
        if (instance == null) { // Первая проверка
            synchronized (Singleton.class) {
                if (instance == null) { // Вторая проверка внутри синхронизированного блока
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## Builder
```java
public class User {
    private final String firstName; // Обязательное поле
    private final String lastName;  // Обязательное поле
    private int age;                // Необязательное поле
    private String email;           // Необязательное поле
    private String address;         // Необязательное поле

    private User(UserBuilder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.email = builder.email;
        this.address = builder.address;
    }

    public static class UserBuilder {
        private final String firstName;
        private final String lastName;
        private int age;
        private String email;
        private String address;

        public UserBuilder(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public UserBuilder age(int age) {
            this.age = age;
            return this;
        }

        public UserBuilder email(String email) {
            this.email = email;
            return this;
        }

        public UserBuilder address(String address) {
            this.address = address;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }

    // Геттеры для полей User
    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }

    public String getAddress() {
        return address;
    }

    @Override
    public String toString() {
        return "User{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

public class Main {
    public static void main(String[] args) {
        // Использование паттерна "Builder" для создания объекта User
        User user = new User.UserBuilder("John", "Doe")
                .age(30)
                .email("john@example.com")
                .address("123 Main St")
                .build();

        System.out.println(user);
    }
}
```

## Dependency Injection

Паттерн `Dependency Injection (DI)` используется для управления зависимостями в объектно-ориентированных приложениях. Этот паттерн позволяет внедрять зависимости в классы вместо того, чтобы создавать их внутри классов, что делает код более гибким и тестируемым. В `Java`, `DI` часто реализуется с использованием конструкторов или методов (сеттеров) для внедрения зависимостей. Вот пример `DI` на Java:

```java
// Пример класса зависимости
public class UserService {
    public void createUser(String username) {
        // Логика создания пользователя
        System.out.println("User created: " + username);
    }
}

// Пример класса, который использует зависимость
public class UserController {
    private final UserService userService;

    // Конструктор для внедрения зависимости через конструктор
    public UserController(UserService userService) {
        this.userService = userService;
    }

    public void registerUser(String username) {
        // Использование зависимости
        userService.createUser(username);
    }
}

public class Main {
    public static void main(String[] args) {
        // Создание зависимости
        UserService userService = new UserService();

        // Создание класса, который использует зависимость
        UserController userController = new UserController(userService);

        // Вызов метода, который использует зависимость
        userController.registerUser("JohnDoe");
    }
}
```

В этом примере `UserController` зависит от `UserService`, и зависимость передается через конструктор `UserController`. Это позволяет нам заменить `UserService` другой реализацией (например, мок-объектом для тестирования) без изменения кода `UserController`. Этот подход делает код более гибким и облегчает тестирование, так как мы можем легко заменить реальные зависимости на фиктивные (`mock`) объекты при модульном тестировании.

##

# Structural

# Behavioral

# Concurrency