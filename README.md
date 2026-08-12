# flutter_design_patterns

Design patterns written in Dart, one runnable Flutter app per pattern.

So far there is one: the decorator, in `decorator_pattern_flutter/`.

## The decorator

The app is a menu. You pick a main course and then add side dishes to it,
and the price and the description both update. The point is that no class
knows the full combination ahead of time.

`Meal` is the abstract type with a `description` and a `cost()`. `Burger`,
`Pizza` and `HotDog` implement it directly and return a fixed price.
`SideDish` also extends `Meal` but holds a `Meal` of its own, which is the
decorator part. `Salad`, `Fries` and `Drink` extend `SideDish` and each one
adds its price to whatever it wraps:

```dart
class Salad extends SideDish {
  Salad({required Meal meal}) : super(meal: meal);

  @override
  String get description => "${meal.description}\nSalad";

  @override
  double cost() => meal.cost() + 3.50;
}
```

A burger with salad and fries is `Fries(meal: Salad(meal: Burger()))`.
Adding a fourth side dish means one new class and no change to the other
six. `MenuBloc` holds the current chain and rebuilds it as items are
toggled.

## Running it

This one is from 2022 and its SDK constraint is `>=2.18.2 <3.0.0`, so it
needs a Dart 2 toolchain. The pattern is the part worth reading; the app
around it is a thin shell over `flutter_bloc` 8.

```sh
cd decorator_pattern_flutter
flutter pub get
flutter run
```

## License

MIT. See [LICENSE](LICENSE).
