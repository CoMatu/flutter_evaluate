## Объясните, как паттерн Builder улучшает читаемость кода.

Паттерн Builder в Dart используется для улучшения читаемости кода путем отделения процесса создания объекта от его представления. Этот паттерн позволяет создавать сложные объекты через последовательность простых шагов, что делает код более понятным и легко поддерживаемым.

Вот пример использования паттерна Builder в Dart:

```dart
class UserBuilder {
  String firstName;
  String lastName;
  int age;

  UserBuilder withFirstName(String firstName) => this.firstName = firstName;

  UserBuilder withLastName(String lastName) => this.lastName = lastName;

  UserBuilder withAge(int age) => this.age = age;

  User build() => User(firstName, lastName, age);
}

class User {
  final String firstName;
  final String lastName;
  final int age;

  User(this.firstName, this.lastName, this.age);
}

void main() {
  User user = UserBuilder()
    .withFirstName("John")
    .withLastName("Doe")
    .withAge(30)
    .build();

  print(user); // Выведет User(firstName: John, lastName: Doe, age: 30)
}
```

В этом примере мы создаем класс `UserBuilder`, который предоставляет методы для настройки свойств объекта `User`. Каждый метод возвращает ссылку на текущий объект `UserBuilder`, позволяя нам добавлять дополнительные свойства в цепочке вызовов методов. Когда все свойства настроены, мы вызываем метод `build`, который создает и возвращает готовый объект `User`.

Этот подход позволяет нам сосредоточиться на создании объекта шаг за шагом, не беспокоясь о том, как он будет представлен. Кроме того, такой код легче поддерживать, так как мы можем изменять или добавлять новые свойства, не затрагивая существующий код.