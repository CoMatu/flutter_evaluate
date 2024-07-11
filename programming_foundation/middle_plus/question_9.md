## Как реализовать паттерн Mediator в Flutter?

Паттерн **Mediator** (Посредник) используется для уменьшения связанности между компонентами, облегчая их взаимодействие через посредника. В контексте Flutter, паттерн Mediator может быть полезен для управления взаимодействием между виджетами или состояниями.

### Реализация паттерна Mediator в Flutter

Рассмотрим пример, в котором у нас есть несколько виджетов, взаимодействующих друг с другом через посредника.

### Шаг 1: Создание интерфейса посредника (Mediator)

Сначала определим интерфейс посредника, который будет использоваться для общения между виджетами.

```dart
abstract class Mediator {
  void notify(Object sender, String event);
}
```

### Шаг 2: Реализация конкретного посредника

Теперь реализуем конкретный посредник, который будет управлять взаимодействием между виджетами.

```dart
class ConcreteMediator implements Mediator {
  late ButtonWidget _button;
  late TextWidget _text;

  void setButton(ButtonWidget button) {
    _button = button;
  }

  void setText(TextWidget text) {
    _text = text;
  }

  @override
  void notify(Object sender, String event) {
    if (event == "buttonClicked") {
      _text.updateText("Button was clicked!");
    }
  }
}
```

### Шаг 3: Создание компонентов, взаимодействующих через посредника

#### Компонент кнопки (ButtonWidget)

```dart
import 'package:flutter/material.dart';

class ButtonWidget extends StatelessWidget {
  final Mediator mediator;

  ButtonWidget(this.mediator);

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        mediator.notify(this, "buttonClicked");
      },
      child: Text("Click Me"),
    );
  }
}
```

#### Компонент текста (TextWidget)

```dart
import 'package:flutter/material.dart';

class TextWidget extends StatefulWidget {
  final Mediator mediator;

  TextWidget(this.mediator);

  @override
  _TextWidgetState createState() => _TextWidgetState();
}

class _TextWidgetState extends State<TextWidget> {
  String _text = "Initial Text";

  void updateText(String newText) {
    setState(() {
      _text = newText;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Text(_text);
  }
}
```

### Шаг 4: Сборка виджетов в приложении

Теперь создадим основное приложение, в котором компоненты будут взаимодействовать через посредника.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Mediator Pattern Example"),
        ),
        body: MediatorExample(),
      ),
    );
  }
}

class MediatorExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    ConcreteMediator mediator = ConcreteMediator();

    ButtonWidget button = ButtonWidget(mediator);
    TextWidget text = TextWidget(mediator);

    mediator.setButton(button);
    mediator.setText(text);

    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        button,
        SizedBox(height: 20),
        text,
      ],
    );
  }
}
```

### Заключение

В этом примере мы создали простое приложение Flutter, использующее паттерн Mediator для управления взаимодействием между кнопкой и текстом. Посредник (ConcreteMediator) управляет обменом данными между компонентами (ButtonWidget и TextWidget), что уменьшает связанность и упрощает управление взаимодействием.

Этот подход может быть расширен для более сложных сценариев, где требуется управление большим количеством взаимодействующих компонентов. Паттерн Mediator помогает улучшить масштабируемость и поддерживаемость кода за счет централизованного управления взаимодействиями.