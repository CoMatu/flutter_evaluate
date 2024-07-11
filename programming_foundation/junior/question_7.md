## Приведите пример использования паттерна Decorator.

Паттерн Decorator позволяет динамически добавлять новые поведения объектам, оборачивая их в классы-декораторы. Это полезно, когда мы хотим расширить функциональность объектов без изменения их исходного кода.

Ниже приведен пример использования паттерна Decorator в Dart:

### Определение абстрактного компонента
```dart
abstract class Coffee {
  String getDescription();
  double cost();
}
```

### Реализация базового компонента
```dart
class SimpleCoffee implements Coffee {
  @override
  String getDescription() {
    return "Simple coffee";
  }

  @override
  double cost() {
    return 5.0;
  }
}
```

### Определение абстрактного декоратора
```dart
abstract class CoffeeDecorator implements Coffee {
  final Coffee coffee;

  CoffeeDecorator(this.coffee);

  @override
  String getDescription() {
    return coffee.getDescription();
  }

  @override
  double cost() {
    return coffee.cost();
  }
}
```

### Реализация конкретных декораторов
#### Добавление молока
```dart
class MilkDecorator extends CoffeeDecorator {
  MilkDecorator(Coffee coffee) : super(coffee);

  @override
  String getDescription() {
    return "${coffee.getDescription()}, Milk";
  }

  @override
  double cost() {
    return coffee.cost() + 1.5;
  }
}
```

#### Добавление сахара
```dart
class SugarDecorator extends CoffeeDecorator {
  SugarDecorator(Coffee coffee) : super(coffee);

  @override
  String getDescription() {
    return "${coffee.getDescription()}, Sugar";
  }

  @override
  double cost() {
    return coffee.cost() + 0.5;
  }
}
```

### Использование паттерна Decorator
```dart
void main() {
  Coffee coffee = SimpleCoffee();
  print("Description: ${coffee.getDescription()}");
  print("Cost: ${coffee.cost()}");

  coffee = MilkDecorator(coffee);
  print("Description: ${coffee.getDescription()}");
  print("Cost: ${coffee.cost()}");

  coffee = SugarDecorator(coffee);
  print("Description: ${coffee.getDescription()}");
  print("Cost: ${coffee.cost()}");
}
```

### Результат выполнения кода
```
Description: Simple coffee
Cost: 5.0
Description: Simple coffee, Milk
Cost: 6.5
Description: Simple coffee, Milk, Sugar
Cost: 7.0
```

В этом примере мы создали простой кофе, а затем добавили к нему молоко и сахар с помощью декораторов. Паттерн Decorator позволяет гибко добавлять новые компоненты и изменять поведение объектов во время выполнения программы.