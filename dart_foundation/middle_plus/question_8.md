## Как работают метапрограммирование и аннотации в Dart?

Метапрограммирование в Dart позволяет писать код, который может анализировать и изменять другие части кода во время выполнения или компиляции. Это достигается с помощью аннотаций и зеркал (mirrors). 

Аннотации (annotations) используются для добавления метаданных к вашему коду, которые могут быть использованы для различных целей, таких как генерация кода, валидация или модификация поведения программы.

### Аннотации в Dart

Аннотации в Dart - это обычные классы, которые используются для добавления метаданных к элементам программы, таким как классы, методы, поля и функции.

#### Создание и использование аннотаций

```dart
// Определение аннотации
class MyAnnotation {
  final String description;
  const MyAnnotation(this.description);
}

// Использование аннотации
@MyAnnotation('This is a sample class')
class MyClass {
  @MyAnnotation('This is a sample method')
  void myMethod() {
    print('Hello, Dart!');
  }
}

void main() {
  var myClassInstance = MyClass();
  myClassInstance.myMethod();
}
```

### Метапрограммирование с помощью зеркал (mirrors)

Dart предоставляет библиотеку `dart:mirrors` для отражения (reflection), которая позволяет вашему коду анализировать структуру программы во время выполнения.

#### Пример использования `dart:mirrors`

```dart
import 'dart:mirrors';

void main() {
  var myClassInstance = MyClass();
  reflectInstance(myClassInstance);
}

class MyClass {
  void sayHello() {
    print('Hello, Dart!');
  }
}

void reflectInstance(Object instance) {
  // Получаем отражение объекта
  InstanceMirror instanceMirror = reflect(instance);
  
  // Получаем отражение класса
  ClassMirror classMirror = instanceMirror.type;
  
  // Перебираем методы класса
  classMirror.declarations.forEach((symbol, declaration) {
    if (declaration is MethodMirror) {
      print('Found method: ${MirrorSystem.getName(symbol)}');
    }
  });
}
```

Этот пример показывает, как использовать отражения для анализа структуры класса и получения списка методов класса во время выполнения.

### Генерация кода

Генерация кода - это еще один аспект метапрограммирования в Dart. Она часто используется в сочетании с аннотациями. Инструменты для генерации кода, такие как `build_runner` и `source_gen`, позволяют автоматически создавать код на основе аннотаций.

#### Пример использования `build_runner` и `source_gen`

1. **Добавьте зависимости в `pubspec.yaml`**:

```yaml
dependencies:
  source_gen: ^1.0.0
  build: ^2.0.0

dev_dependencies:
  build_runner: ^2.0.0
```

2. **Создайте аннотацию**:

```dart
// lib/my_annotation.dart
class MyAnnotation {
  final String description;
  const MyAnnotation(this.description);
}
```

3. **Создайте генератор кода**:

```dart
// lib/my_generator.dart
import 'package:build/build.dart';
import 'package:source_gen/source_gen.dart';
import 'package:my_package/my_annotation.dart';
import 'dart:async';

class MyGenerator extends GeneratorForAnnotation<MyAnnotation> {
  @override
  FutureOr<String> generateForAnnotatedElement(
      Element element, ConstantReader annotation, BuildStep buildStep) {
    final description = annotation.read('description').stringValue;
    return '''
// GENERATED CODE - DO NOT MODIFY BY HAND
// Description: $description

void generatedFunction() {
  print('$description');
}
''';
  }
}
```

4. **Настройте `build.yaml`**:

```yaml
# build.yaml
targets:
  $default:
    builders:
      my_package|my_generator:
        enabled: true
```

5. **Запустите генерацию кода**:

```sh
dart run build_runner build
```

Этот процесс автоматически сгенерирует файл с кодом на основе аннотаций, которые вы добавили к вашим классам и методам.

### Заключение

Метапрограммирование и аннотации в Dart предоставляют мощные инструменты для создания гибкого и динамичного кода. Аннотации позволяют добавлять метаданные, которые могут быть использованы для различных целей, а отражения и генерация кода предоставляют механизмы для анализа и модификации кода во время выполнения и компиляции.