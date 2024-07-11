## Приведите пример применения паттерна Strategy

Паттерн Strategy используется для того, чтобы отделить алгоритм от его реализации. Это позволяет легко менять поведение системы без необходимости изменять её структуру.

В Dart Flutter паттерн Strategy может быть использован, например, для создания различных способов навигации между экранами приложения. Вместо того чтобы каждый раз писать новый код для каждого способа навигации, можно создать абстрактный класс NavigatorStrategy, который будет содержать общий интерфейс для всех стратегий навигации. Затем можно определить конкретные классы, такие как SwipeNavigatorStrategy, BackButtonNavigatorStrategy и т.д., которые будут реализовывать этот интерфейс.

Вот пример кода, который демонстрирует использование паттерна Strategy в Dart Flutter:

```dart
abstract class NavigatorStrategy {
  void navigateToScreen(String screenName);
}

class SwipeNavigatorStrategy implements NavigatorStrategy {
  @override
  void navigateToScreen(String screenName) {
    // Код для навигации с помощью свайпа
  }
}

class BackButtonNavigatorStrategy implements NavigatorStrategy {
  @override
  void navigateToScreen(String screenName) {
    // Код для навигации с помощью кнопки "Назад"
  }
}

// Класс, который использует стратегию навигации
class MyApp {
  final NavigatorStrategy navigatorStrategy;

  MyApp(this.navigatorStrategy);

  void navigate() {
    navigatorStrategy.navigateToScreen('screenName');
  }
}

void main() {
  var swipeNavigator = SwipeNavigatorStrategy();
  var backButtonNavigator = BackButtonNavigatorStrategy();

  var appWithSwipeNavigation = MyApp(swipeNavigator);
  appWithSwipeNavigation.navigate(); // Использует свайп для навигации

  var appWithBackButtonNavigation = MyApp(backButtonNavigator);
  appWithBackButtonNavigation.navigate(); // Использует кнопку "Назад" для навигации
}
```

В этом примере мы создаем абстрактный класс `NavigatorStrategy`, который определяет общий интерфейс для всех стратегий навигации. Затем мы определяем два конкретных класса, `SwipeNavigatorStrategy` и `BackButtonNavigatorStrategy`, которые реализуют этот интерфейс. В классе `MyApp` мы используем стратегию навигации, которую передаем в конструктор. Таким образом, мы можем легко изменить способ навигации, просто изменив экземпляр стратегии, который мы передаем в `MyApp`.