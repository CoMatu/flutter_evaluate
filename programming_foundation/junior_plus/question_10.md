## Как реализовать паттерн Command в Dart?

Паттерн **Command** (Команда) является поведенческим паттерном проектирования, который превращает запросы в объекты, позволяя передавать их как аргументы при вызове методов, ставить запросы в очередь или журналировать их. Паттерн Command позволяет отделить объект, инициирующий операцию, от объекта, который фактически выполняет эту операцию.

### Основные элементы паттерна Command:

1. **Command**: Интерфейс или абстрактный класс, который объявляет метод для выполнения команды.
2. **ConcreteCommand**: Конкретная реализация команды, которая выполняет действие, вызывая соответствующий метод получателя.
3. **Receiver**: Класс, который выполняет фактические действия, необходимые для выполнения команды.
4. **Invoker**: Класс, который хранит команду и может вызвать её выполнение.
5. **Client**: Класс, который создаёт объекты команд и связывает их с получателями.

### Пример реализации паттерна Command в Dart

#### Шаг 1: Определение интерфейса команды

```dart
abstract class Command {
  void execute();
}
```

#### Шаг 2: Создание классов получателей (Receiver)

```dart
class Light {
  void turnOn() {
    print("The light is on");
  }

  void turnOff() {
    print("The light is off");
  }
}
```

#### Шаг 3: Создание конкретных команд (ConcreteCommand)

```dart
class TurnOnCommand implements Command {
  final Light light;

  TurnOnCommand(this.light);

  @override
  void execute() {
    light.turnOn();
  }
}

class TurnOffCommand implements Command {
  final Light light;

  TurnOffCommand(this.light);

  @override
  void execute() {
    light.turnOff();
  }
}
```

#### Шаг 4: Создание инициатора (Invoker)

```dart
class RemoteControl {
  Command? _command;

  void setCommand(Command command) {
    _command = command;
  }

  void pressButton() {
    _command?.execute();
  }
}
```

#### Шаг 5: Использование паттерна Command

```dart
void main() {
  // Создание получателя
  Light livingRoomLight = Light();

  // Создание конкретных команд
  Command turnOn = TurnOnCommand(livingRoomLight);
  Command turnOff = TurnOffCommand(livingRoomLight);

  // Создание инициатора
  RemoteControl remote = RemoteControl();

  // Включение света
  remote.setCommand(turnOn);
  remote.pressButton(); // Output: The light is on

  // Выключение света
  remote.setCommand(turnOff);
  remote.pressButton(); // Output: The light is off
}
```

### Дополнительные возможности

#### Шаг 6: Реализация команды отмены (Undo)

Для добавления возможности отмены команды можно расширить интерфейс команды.

```dart
abstract class Command {
  void execute();
  void undo();
}

class TurnOnCommand implements Command {
  final Light light;

  TurnOnCommand(this.light);

  @override
  void execute() {
    light.turnOn();
  }

  @override
  void undo() {
    light.turnOff();
  }
}

class TurnOffCommand implements Command {
  final Light light;

  TurnOffCommand(this.light);

  @override
  void execute() {
    light.turnOff();
  }

  @override
  void undo() {
    light.turnOn();
  }
}

class RemoteControl {
  Command? _command;

  void setCommand(Command command) {
    _command = command;
  }

  void pressButton() {
    _command?.execute();
  }

  void pressUndoButton() {
    _command?.undo();
  }
}

void main() {
  Light livingRoomLight = Light();

  Command turnOn = TurnOnCommand(livingRoomLight);
  Command turnOff = TurnOffCommand(livingRoomLight);

  RemoteControl remote = RemoteControl();

  // Включение света
  remote.setCommand(turnOn);
  remote.pressButton(); // Output: The light is on
  remote.pressUndoButton(); // Output: The light is off

  // Выключение света
  remote.setCommand(turnOff);
  remote.pressButton(); // Output: The light is off
  remote.pressUndoButton(); // Output: The light is on
}
```

### Заключение

Паттерн Command полезен для реализации задач, связанных с логированием операций, отменой и повторным выполнением операций, а также для создания очередей команд. Он отделяет объекты, инициирующие операции, от объектов, которые их выполняют, что делает систему более гибкой и расширяемой. В Dart паттерн Command легко реализуется с использованием интерфейсов и классов, как показано в приведенном примере.