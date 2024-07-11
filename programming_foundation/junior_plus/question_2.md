## Объясните Liskov Substitution Principle на примере.

Принцип подстановки Барбары Лисков (Liskov Substitution Principle, LSP) является одним из пяти принципов SOLID и утверждает, что объекты в программе должны быть заменяемыми на экземпляры их подтипов без изменения правильности выполнения программы. Другими словами, если класс S является подтипом класса T, то объекты типа T могут быть заменены объектами типа S без изменения поведения программы.

### Основная идея LSP:
- Подклассы должны дополнять, а не изменять поведение базовых классов.
- Методы подклассов должны сохранять контракт, установленный методами базового класса.

### Пример нарушения LSP

Рассмотрим простой пример с базовым классом `Bird` и подклассами `Eagle` и `Penguin`.

```dart
class Bird {
  void fly() {
    print("Flying");
  }
}

class Eagle extends Bird {
  @override
  void fly() {
    print("Eagle is flying high");
  }
}

class Penguin extends Bird {
  @override
  void fly() {
    // Пингвины не умеют летать
    throw Exception("Penguins can't fly");
  }
}
```

В этом примере `Penguin` нарушает принцип LSP, так как он не может летать, несмотря на то, что является подклассом `Bird`. Если в коде используется экземпляр `Bird`, который может быть заменен `Penguin`, программа сломается.

### Пример соблюдения LSP

Чтобы соблюдать принцип LSP, нужно пересмотреть иерархию классов. Можно создать интерфейс `Flyable` для летающих птиц и интерфейс `Bird` для всех птиц.

```dart
abstract class Bird {
  void eat();
}

abstract class Flyable {
  void fly();
}

class Eagle extends Bird implements Flyable {
  @override
  void eat() {
    print("Eagle is eating");
  }

  @override
  void fly() {
    print("Eagle is flying high");
  }
}

class Penguin extends Bird {
  @override
  void eat() {
    print("Penguin is eating");
  }

  // Penguins do not implement Flyable
}
```

Теперь `Penguin` не нарушает принцип LSP, так как он больше не обязан реализовывать метод `fly`, и заменяемость объектов типа `Bird` сохраняется.

### Пример на Dart

#### Базовый класс и подклассы

```dart
abstract class Bird {
  void eat();
}

abstract class Flyable {
  void fly();
}

class Eagle extends Bird implements Flyable {
  @override
  void eat() {
    print("Eagle is eating");
  }

  @override
  void fly() {
    print("Eagle is flying high");
  }
}

class Penguin extends Bird {
  @override
  void eat() {
    print("Penguin is eating");
  }
}
```

#### Пример использования классов

```dart
void main() {
  Bird eagle = Eagle();
  Bird penguin = Penguin();

  eagle.eat(); // Output: Eagle is eating
  penguin.eat(); // Output: Penguin is eating

  Flyable flyableEagle = Eagle();
  flyableEagle.fly(); // Output: Eagle is flying high

  // Flyable flyablePenguin = Penguin(); // Ошибка: Penguin не реализует Flyable
}
```

В этом примере мы используем интерфейс `Flyable` для летающих птиц и интерфейс `Bird` для всех птиц. Это позволяет нам соблюдать принцип LSP и избежать ситуаций, когда подклассы не соответствуют поведению базового класса.

### Заключение

Принцип подстановки Лисков помогает создавать более гибкие и устойчивые к изменениям системы, обеспечивая правильное использование наследования. Соблюдение LSP требует, чтобы подклассы полностью соответствовали контракту своих базовых классов и не изменяли их поведение. Это способствует созданию кода, который легче понимать, тестировать и поддерживать.