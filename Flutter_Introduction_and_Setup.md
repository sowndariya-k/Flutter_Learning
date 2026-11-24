# Flutter Introduction and Environment Setup

Notes for anyone starting Flutter from scratch. It covers what Flutter is, how to install it, how a project is laid out, and how the default counter app works.

## Contents

1. [What is Flutter](#1-what-is-flutter)
2. [Dart and Flutter](#2-dart-and-flutter)
3. [Tools required](#3-tools-required)
4. [Installation on Windows](#4-installation-on-windows)
5. [Create an emulator](#5-create-an-emulator)
6. [Verify the setup](#6-verify-the-setup)
7. [Create the first project](#7-create-the-first-project)
8. [Project skeleton](#8-project-skeleton)
9. [The lib folder](#9-the-lib-folder)
10. [main.dart](#10-maindart)
11. [Widgets and widget tree](#11-widgets-and-widget-tree)
12. [StatelessWidget vs StatefulWidget](#12-statelesswidget-vs-statefulwidget)
13. [The default counter app](#13-the-default-counter-app)
14. [Run the app](#14-run-the-app)
15. [Hot Reload and Hot Restart](#15-hot-reload-and-hot-restart)
16. [Useful commands](#16-useful-commands)
17. [Practice](#17-practice)
18. [Links](#18-links)

---

## 1. What is Flutter

Flutter is an open-source UI framework from Google. One codebase can be used to build apps for Android, iOS, web, Windows, macOS and Linux.

Why it is popular:

- One codebase for many platforms
- Hot Reload shows code changes quickly
- The UI is built from small reusable widgets
- Release builds are compiled to native code, so performance is good

## 2. Dart and Flutter

- Dart is the programming language.
- Flutter is the framework written in Dart.

Dart gives us variables, conditions, loops, functions and classes. Flutter gives us the UI pieces like Text, Button, Image, Row and Column.

Dart comes bundled with the Flutter SDK, so it does not need a separate install.

## 3. Tools required

| Tool | Why |
|---|---|
| Flutter SDK | The framework and the `flutter` command |
| Git | Source control, also used by Flutter tooling |
| VS Code | Editor to write the code |
| Android Studio | Installs the Android SDK and creates emulators |
| Android Emulator | A virtual phone to run the app |

Flutter SDK and Android Studio are two separate installations. Even if we write code in VS Code, Android Studio is still needed for the Android SDK and emulator.

## 4. Installation on Windows

1. **Git**: install from [git-scm.com](https://git-scm.com/downloads), then check with `git --version`.
2. **VS Code**: install from [code.visualstudio.com](https://code.visualstudio.com/).
3. **Flutter SDK**: the official guide recommends installing it through the Flutter extension in VS Code. Manual install also works: download the zip, extract it to a path without spaces (for example `C:\src\flutter`), and add `C:\src\flutter\bin` to the PATH. Follow the steps here: [Install Flutter](https://docs.flutter.dev/install).
4. **Android Studio**: install from [developer.android.com/studio](https://developer.android.com/studio). On first launch let it download the Android SDK. In SDK Manager, under SDK Tools, also install **Android SDK Command-line Tools**, otherwise the license step fails.

Close and reopen PowerShell after the install, then check:

```powershell
flutter --version
```

## 5. Create an emulator

1. Open Android Studio, then Device Manager.
2. Click Create Virtual Device.
3. Pick a device (for example Pixel) and click Next.
4. Select a system image and download it if needed.
5. Click Finish, then press the Play button to start it.

Check that Flutter can see it:

```powershell
flutter devices
```

## 6. Verify the setup

```powershell
flutter doctor
```

This lists what is installed and what is missing. If the Android SDK is not detected, point Flutter to it:

```powershell
flutter config --android-sdk "C:\Users\<username>\AppData\Local\Android\Sdk"
```

Then accept the licenses and run doctor again:

```powershell
flutter doctor --android-licenses
flutter doctor
```

The Android toolchain line should now be fine. Warnings for platforms we are not using (for example iOS on Windows) can be ignored.

## 7. Create the first project

```powershell
flutter create my_first_flutter_app
cd my_first_flutter_app
code .
```

Use lowercase letters and underscores for the project name.

## 8. Project skeleton

This is what `flutter create` generates:

```
my_first_flutter_app/
├── android/                 Android native project
├── ios/                     iOS native project
├── web/                     Web files (index.html, icons)
├── windows/                 Windows desktop project
├── linux/                   Linux desktop project
├── macos/                   macOS desktop project
├── lib/
│   └── main.dart            Our Dart code, app starts here
├── test/
│   └── widget_test.dart     Automated tests
├── pubspec.yaml             Packages, assets, fonts
├── pubspec.lock             Exact package versions (auto-generated)
├── analysis_options.yaml    Code analysis / lint rules
├── .gitignore               Files Git should ignore
├── .dart_tool/              Generated, do not edit
└── build/                   Build output, do not edit
```

Some platform folders may not appear depending on which platforms are enabled.

What matters at the start:

| Folder / file | Purpose |
|---|---|
| `lib/` | All our Dart code. We work here every day. |
| `lib/main.dart` | Entry point of the app. |
| `pubspec.yaml` | Add packages, images and fonts here. Run `flutter pub get` after changing it. |
| `test/` | Test files. |
| `android/`, `ios/`, `web/`, etc. | Small native wrapper for each platform. Rarely edited (permissions, app icon, app name). |
| `pubspec.lock` | Locks package versions so everyone gets the same ones. Not edited by hand. |
| `.dart_tool/`, `build/` | Generated by tools. Not committed to Git. |

To begin with, focus only on `lib/`, `pubspec.yaml` and `test/`.

## 9. The lib folder

`lib` is short for library. It is where the application code lives.

Writing a whole app in `main.dart` gets messy quickly, so code is usually split by purpose. For example, in an employee management app:

```
lib/
├── main.dart
├── screens/
│   ├── login_screen.dart
│   ├── home_screen.dart
│   └── employee_screen.dart
├── widgets/
│   ├── custom_button.dart
│   └── custom_textfield.dart
├── models/
│   └── employee.dart
└── services/
    └── api_service.dart
```

| Folder | Holds |
|---|---|
| `screens/` | Full pages |
| `widgets/` | Reusable UI pieces |
| `models/` | Data classes |
| `services/` | API calls and business logic |

These sub-folders are a convention, not a Flutter rule. Only `lib/main.dart` is required.

## 10. main.dart

Execution starts from `main()`.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}
```

- `import 'package:flutter/material.dart'` brings in the Material widgets (Scaffold, AppBar, Text, Icon, buttons).
- `main()` is the entry point of the program.
- `runApp()` starts the app with the given root widget.
- `MyApp` is the root widget.

## 11. Widgets and widget tree

In Flutter, almost everything on screen is a widget.

| Widget | Purpose |
|---|---|
| Text | Shows text |
| Image | Shows an image |
| Icon | Shows an icon |
| Container | Box with size, colour, padding |
| Row | Children side by side |
| Column | Children one below the other |
| Center | Centres its child |
| Scaffold | Basic page structure |
| AppBar | Top bar |
| ElevatedButton | A button |

Widgets are placed inside other widgets, which forms a tree:

```
MaterialApp
 └── Scaffold
      ├── AppBar
      │    └── Text
      ├── Center
      │    └── Column
      │         ├── Text
      │         └── Text
      └── FloatingActionButton
           └── Icon
```

## 12. StatelessWidget vs StatefulWidget

**StatelessWidget** is for UI that does not change on its own, like a label or an icon.

```dart
class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return const Text('Welcome to Flutter');
  }
}
```

**StatefulWidget** is for UI that changes with data, like a counter or a form.

```dart
class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  int counter = 0;

  @override
  Widget build(BuildContext context) {
    return Text('$counter');
  }
}
```

| StatelessWidget | StatefulWidget |
|---|---|
| No changing data of its own | Has a State object holding changing data |
| Static UI | Dynamic UI |
| Cannot call `setState()` for itself | Uses `setState()` to refresh the UI |

## 13. The default counter app

Every new project comes with a counter app. A number is shown on screen and the + button increases it.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('You have pushed the button this many times:'),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

The generated template can look slightly different between Flutter versions, but the logic is the same.

What each part does:

- `MyApp` is the root widget. It is stateless because it never changes.
- `MaterialApp` sets the title, theme and the first screen (`home`).
- `MyHomePage` is stateful because the counter value changes.
- `_counter` stores the current count.
- `_incrementCounter()` adds 1 and calls `setState()`.
- `Scaffold` gives the page structure: app bar, body and floating button.
- `Center` and `Column` place the two texts in the middle, one below the other.
- `FloatingActionButton` calls `_incrementCounter` when pressed.

What happens when the button is tapped:

1. `onPressed` is triggered.
2. `_incrementCounter()` runs and increases `_counter`.
3. `setState()` tells Flutter the data changed.
4. Flutter rebuilds the widget and the new value is shown.

If `_counter++` is done without `setState()`, the value changes in memory but the screen does not update.

Flow of the app:

```
main()
 └── runApp(MyApp)
      └── MaterialApp
           └── MyHomePage
                └── _MyHomePageState.build()
                     └── Scaffold
                          └── FloatingActionButton
                               └── _incrementCounter()
                                    └── setState()
                                         └── UI updates
```

## 14. Run the app

1. Start the emulator from Android Studio.
2. Check the device: `flutter devices`
3. From the project folder, run: `flutter run`

The first build takes a few minutes because Gradle downloads dependencies. Later runs are faster.

To test, change `int _counter = 0;` to `int _counter = 10;`, save, and watch the screen.

## 15. Hot Reload and Hot Restart

| Hot Reload | Hot Restart |
|---|---|
| Applies code changes to the running app | Restarts the app from the beginning |
| Keeps the current state | Resets the state |
| Faster | Slower |

Example: the counter shows 5. After a Hot Reload it still shows 5. After a Hot Restart it goes back to 0.

In the terminal running `flutter run`: `r` for Hot Reload, `R` for Hot Restart, `q` to quit.

## 16. Useful commands

| Command | Use |
|---|---|
| `flutter --version` | Show Flutter and Dart version |
| `flutter doctor` | Check the environment |
| `flutter devices` | List devices and emulators |
| `flutter create name` | Create a project |
| `flutter run` | Run the app |
| `flutter pub get` | Download packages |
| `flutter clean` | Delete build files |
| `flutter analyze` | Check code for problems |
| `flutter test` | Run tests |
| `flutter build apk` | Build an Android APK |

If something breaks after a change, try `flutter clean`, then `flutter pub get`, then `flutter run`.

## 17. Practice

Try these on the counter app:

- Change the app title and the AppBar colour
- Start the counter from 10
- Increase by 2 on each tap (`_counter += 2;`)
- Change the icon to `Icons.add_circle`
- Add an extra Text, an ElevatedButton, a Container and a Row

Questions to check understanding:

- What is the difference between Flutter and Dart?
- What do `main()` and `runApp()` do?
- What is a widget and a widget tree?
- StatelessWidget vs StatefulWidget?
- What does `setState()` do?
- Row vs Column?
- Hot Reload vs Hot Restart?
- What is `pubspec.yaml` used for?

## 18. Links

- [Flutter install guide](https://docs.flutter.dev/install)
- [Flutter learning pathway](https://docs.flutter.dev/learn/pathway)
- [Widget catalog](https://docs.flutter.dev/ui/widgets)
- [Flutter API reference](https://api.flutter.dev/)
- [Dart language tour](https://dart.dev/language)
- [DartPad (run Dart in the browser)](https://dartpad.dev/)
- [GeeksforGeeks: Flutter tutorial](https://www.geeksforgeeks.org/flutter/flutter-tutorial/)
- [GeeksforGeeks: Dart tutorial](https://www.geeksforgeeks.org/dart/dart-tutorial/)

The official docs are the main reference. Third-party tutorials can be older, so check that their code works with the installed Flutter version.
