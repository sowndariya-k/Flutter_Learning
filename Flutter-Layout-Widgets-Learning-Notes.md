# Flutter Layout Widgets: Learning Notes

**Learning Track:** Flutter Development  
**Topic:** Layout Widgets, Alignment, Scrolling and Reusable Widgets  
**Level:** Beginner

---

## 1. What You Will Learn

A Flutter screen is built by placing widgets inside other widgets. This topic teaches **how to control where each widget goes and how much space it takes**.

By the end of these notes you will be able to:

- Explain how Flutter decides the size of a widget.
- Add spacing and styling with Container.
- Arrange widgets using Row and Column.
- Align widgets using MainAxisAlignment and CrossAxisAlignment.
- Share space between widgets using Expanded and Flexible.
- Stop overflow errors using SingleChildScrollView.
- Make a screen work on different screen sizes.
- Build a clean Login Page using a reusable widget.

**Before starting, you should know:** what a widget is, the difference between StatelessWidget and StatefulWidget, and how to run a Flutter app.

---

## 2. Step 1: How Flutter Decides Size

Every layout problem becomes easier if you remember one rule:

> **The parent gives limits. The child picks a size inside those limits. The parent decides the position.**

| Who | What they do |
|---|---|
| Parent | Says "you can be between this size and that size" |
| Child | Chooses its size within those limits |
| Parent | Places the child on the screen |

**What this means for you**

- A widget cannot take any size it likes. It must follow its parent.
- A widget does not decide its own position. The parent does.
- The same widget can look different inside a Row, a Column or a Center, because each gives different limits.

When a layout looks wrong, ask: **"What limits is the parent giving this widget?"**

---

## 3. Step 2: Container

Container is a box. It can add size, spacing, colour, borders and rounded corners around a widget.

| Property | What it does |
|---|---|
| `width`, `height` | Fixed size of the box |
| `padding` | Space **inside** the box |
| `margin` | Space **outside** the box |
| `color` | Background colour |
| `decoration` | Border, rounded corners, shadow, gradient |
| `alignment` | Position of the child inside the box |
| `child` | The widget inside the box |

### Padding and Margin

```text
+---------------------------------+
|            MARGIN               |   <- space outside
|   +-------------------------+   |
|   |   +-----------------+   |   |
|   |   |    PADDING      |   |   |   <- space inside
|   |   |   +---------+   |   |   |
|   |   |   |  CHILD  |   |   |   |
|   |   |   +---------+   |   |   |
|   |   +-----------------+   |   |
|   +-------------------------+   |
+---------------------------------+
```

### Remember

- No child and no size: the Container fills all the space it is given.
- Has a child and no size: the Container shrinks to fit the child.
- You **cannot use `color` and `decoration` together**. Put the colour inside `BoxDecoration(color: ...)`.
- If you only need space, use `Padding`. If you only need a gap, use `SizedBox`. They are simpler than Container.

---

## 4. Step 3: Row and Column

Both place a list of `children`. Only the direction is different.

| Widget | Direction | Main axis | Cross axis |
|---|---|---|---|
| Column | Top to bottom | Vertical ↕ | Horizontal ↔ |
| Row | Left to right | Horizontal ↔ | Vertical ↕ |

```text
        Column                          Row

        [ A ]                    [ A ] [ B ] [ C ]
        [ B ]
        [ C ]
```

The most important point: **the main axis changes depending on the widget.** In a Column it is vertical. In a Row it is horizontal. All alignment settings follow this.

### Good to know

- A Column takes all available height. A Row takes all available width.
- You can put a Row inside a Column (and the other way) to build any screen.
- They do **not scroll**. If children are too big, you get an overflow error.

---

## 5. Step 4: Alignment

### 5.1 MainAxisAlignment (along the main axis)

Example: a Row with three boxes A, B and C.

```text
start         |[A][B][C]                |
end           |                [A][B][C]|
center        |        [A][B][C]        |
spaceBetween  |[A]          [B]       [C]|
spaceAround   |  [A]      [B]      [C]  |
spaceEvenly   |   [A]     [B]     [C]   |
```

| Value | Meaning |
|---|---|
| `start` | Children at the start (default) |
| `end` | Children at the end |
| `center` | Children in the middle |
| `spaceBetween` | Equal gaps between children, none at the edges |
| `spaceAround` | Equal space around each child |
| `spaceEvenly` | All gaps, including edges, are equal |

### 5.2 CrossAxisAlignment (across the main axis)

In a Column this controls left–right position. In a Row it controls top–bottom position.

| Value | Meaning |
|---|---|
| `start` | Align to the start |
| `end` | Align to the end |
| `center` | Align to the centre (default) |
| `stretch` | Make children fill the full width (in a Column) or height (in a Row) |
| `baseline` | Align text baselines |

### 5.3 MainAxisSize

| Value | Meaning |
|---|---|
| `MainAxisSize.max` | Take all available space (default) |
| `MainAxisSize.min` | Take only the space the children need |

> **Tip:** Alignment only shows a difference when there is **extra space**. If the Row or Column is exactly as big as its children, nothing moves.

---

## 6. Step 5: Expanded and Flexible

These two widgets work **only as a direct child of Row or Column**. They decide how leftover space is shared.

### Expanded

Fills all the remaining space. If many children use Expanded, space is shared using the `flex` number.

```text
Three Expanded children with flex 1 : 2 : 1

|[ A ][       B       ][ C ]|
   25%       50%        25%
```

```dart
Row(
  children: [
    Expanded(flex: 1, child: Container(color: Colors.red)),
    Expanded(flex: 2, child: Container(color: Colors.blue)),
    Expanded(flex: 1, child: Container(color: Colors.green)),
  ],
)
```

### Flexible

Also shares space, but the child **may stay smaller** than the space offered.

| | Expanded | Flexible |
|---|---|---|
| Must fill the space? | Yes | No |
| Uses `flex`? | Yes | Yes |
| Same as | `Flexible(fit: FlexFit.tight)` | `Flexible(fit: FlexFit.loose)` |

**Which one to use**

- Widget should take whatever is left: **Expanded**.
- Widget should share space but stay only as big as it needs: **Flexible**.
- Common example: a TextField beside a button. Put the TextField in `Expanded` so it stretches, and the button keeps its normal size.

---

## 7. Step 6: Scrolling with SingleChildScrollView

### The problem

When content is taller than the screen (for example, when the keyboard opens), Flutter shows a yellow and black striped bar and this message:

`A RenderFlex overflowed by XX pixels on the bottom.`

A Column cannot scroll by itself.

### The solution

Wrap the Column in `SingleChildScrollView`.

```dart
SingleChildScrollView(
  child: Column(
    children: [ /* widgets */ ],
  ),
)
```

### Remember

- Default direction is vertical. Use `scrollDirection: Axis.horizontal` for a Row.
- Use it for **a small, fixed number of widgets** such as forms and login screens.
- For long lists, use ListView instead. It only builds items that are visible.
- Do not put `Expanded` directly inside a scrollable Column. The scroll view gives unlimited height, and Expanded cannot share unlimited space.

---

## 8. Step 7: Responsive UI Basics

Phones, tablets and browser windows all have different sizes. A responsive screen adjusts to them instead of using fixed numbers everywhere.

| Tool | What it does | When to use |
|---|---|---|
| `MediaQuery.of(context).size` | Gives the full screen width and height | Sizing as a fraction of the screen |
| `LayoutBuilder` | Gives the space the parent offers (`maxWidth`, `maxHeight`) | Changing layout based on available space |
| `Expanded` / `Flexible` | Shares space in proportion | Avoiding fixed widths |
| `SafeArea` | Keeps content away from notch and system bars | Top and bottom of screens |
| `ConstrainedBox` | Sets a maximum or minimum size | Stopping forms from getting too wide |

### Simple rules

1. Avoid fixed widths like `width: 350`.
2. Set a maximum width for forms. A form that is 1000 px wide on a tablet looks wrong.
3. Allow scrolling on any screen that has input fields.
4. Use SafeArea.
5. Test on a small phone, a large phone and a wide window.

**MediaQuery or LayoutBuilder?**

- MediaQuery asks: "How big is the **screen**?"
- LayoutBuilder asks: "How big is the **space my parent gave me**?"

---

## 9. Step 8: Reusable Widgets

If you write the same UI again and again, create your own widget once and reuse it.

| Without a reusable widget | With a reusable widget |
|---|---|
| Same code repeated | Written once, used many times |
| A change must be made in every place | One change updates everything |
| Long, confusing `build()` | Short, clear `build()` |

### How to design one

- Make it a **StatelessWidget** if it does not keep changing data itself. Keep the state in the parent.
- Pass changing values (`label`, `controller`) through the **constructor**.
- Give optional values a **default** (`obscureText = false`).
- Mark must-have values as `required`.

### When to split a widget out

- The same UI is used two or more times.
- A `build()` method becomes too long to read.
- A section has a clear purpose (header, input field, card).

### Recommended folder structure

```text
lib/
│
├── main.dart
│
├── screens/
│   └── login_page.dart
│
└── widgets/
    └── custom_text_field.dart
```

- `main.dart`: where the app starts.
- `login_page.dart`: the login screen.
- `custom_text_field.dart`: the reusable input field.

---

## 10. Mini Project: Structured Login Page

### Goal

Use everything above to build a clean, scrollable, responsive Login Page.

### Requirements

1. Form stays in the centre of the screen.
2. Page scrolls when the keyboard opens.
3. Form does not become too wide on large screens.
4. Heading and subheading are centred.
5. Email and password use one reusable `CustomTextField`.
6. Login button fills the full width.
7. A "Don't have an account? Sign Up" row sits below the button.

### Preview

```text
--------------------------------

           Welcome Back
      Login to your account

    [ Email Address            ]
    [ Password              👁 ]

    [          Login          ]

   Don't have an account? Sign Up

--------------------------------
```

### Widget tree

```text
LoginPage (StatefulWidget)
    └── Scaffold
         └── SafeArea
              └── Center
                   └── SingleChildScrollView
                        └── ConstrainedBox (max width 400)
                             └── Column (stretch, size min)
                                  ├── Text (Heading)
                                  ├── Text (Subheading)
                                  ├── CustomTextField (Email)
                                  ├── CustomTextField (Password)
                                  ├── ElevatedButton (Login)
                                  └── Row
                                       ├── Text
                                       └── TextButton (Sign Up)
```

### Why each widget is used

| Widget | Reason |
|---|---|
| SafeArea | Keeps content clear of the notch and status bar |
| Center | Puts the form in the middle |
| SingleChildScrollView | Prevents overflow when the keyboard opens |
| ConstrainedBox | Limits the form to 400 px wide |
| Column with `mainAxisSize.min` | Column is only as tall as its content, so Center can centre it |
| `CrossAxisAlignment.stretch` | Makes fields and button full width |
| SizedBox | Fixed gaps between items |
| Row with `MainAxisAlignment.center` | Centres the "Sign Up" line |

### Code

**`lib/widgets/custom_text_field.dart`**

```dart
import 'package:flutter/material.dart';

class CustomTextField extends StatelessWidget {
  final String label;
  final TextEditingController controller;
  final bool obscureText;
  final TextInputType keyboardType;
  final Widget? suffixIcon;

  const CustomTextField({
    super.key,
    required this.label,
    required this.controller,
    this.obscureText = false,
    this.keyboardType = TextInputType.text,
    this.suffixIcon,
  });

  @override
  Widget build(BuildContext context) {
    return TextField(
      controller: controller,
      obscureText: obscureText,
      keyboardType: keyboardType,
      decoration: InputDecoration(
        labelText: label,
        border: const OutlineInputBorder(),
        suffixIcon: suffixIcon,
      ),
    );
  }
}
```

**`lib/screens/login_page.dart`**

```dart
import 'package:flutter/material.dart';
import '../widgets/custom_text_field.dart';

class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final emailController = TextEditingController();
  final passwordController = TextEditingController();
  bool isPasswordVisible = false;

  void login() {
    final empty = emailController.text.isEmpty ||
        passwordController.text.isEmpty;

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(empty ? "Please enter all fields" : "Login button clicked"),
      ),
    );
  }

  @override
  void dispose() {
    emailController.dispose();
    passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: SingleChildScrollView(
            padding: const EdgeInsets.all(20),
            child: ConstrainedBox(
              constraints: const BoxConstraints(maxWidth: 400),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  const Text(
                    "Welcome Back",
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  const Text("Login to your account", textAlign: TextAlign.center),
                  const SizedBox(height: 30),

                  CustomTextField(
                    label: "Email Address",
                    controller: emailController,
                    keyboardType: TextInputType.emailAddress,
                  ),
                  const SizedBox(height: 15),

                  CustomTextField(
                    label: "Password",
                    controller: passwordController,
                    obscureText: !isPasswordVisible,
                    suffixIcon: IconButton(
                      icon: Icon(isPasswordVisible
                          ? Icons.visibility
                          : Icons.visibility_off),
                      onPressed: () =>
                          setState(() => isPasswordVisible = !isPasswordVisible),
                    ),
                  ),
                  const SizedBox(height: 20),

                  ElevatedButton(onPressed: login, child: const Text("Login")),

                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      const Text("Don't have an account?"),
                      TextButton(onPressed: () {}, child: const Text("Sign Up")),
                    ],
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

**Note:** This is a UI-only example. It does not check users against a backend or validate credentials.

---

## 11. Common Errors and Fixes

| Error | Why it happens | Fix |
|---|---|---|
| `A RenderFlex overflowed by ... pixels` | Children are bigger than the available space | Use SingleChildScrollView, or Expanded/Flexible on the big child |
| `Incorrect use of ParentDataWidget` | Expanded/Flexible is not a direct child of Row or Column | Move it directly inside the Row or Column |
| `Vertical viewport was given unbounded height` | A scrollable (like ListView) is inside a Column with no size limit | Wrap it in Expanded or give it a fixed height |
| `BoxConstraints forces an infinite width/height` | A widget wanting maximum size is placed where space is unlimited | Give it a size with SizedBox, or use Expanded |
| Container error with `color` and `decoration` | Both used together | Put the colour inside `BoxDecoration` |
| `mainAxisAlignment` does nothing | No extra space on the main axis | Check `mainAxisSize` or give more room |

---

## 12. Learning Resources

| Resource | Link |
|---|---|
| Flutter Layouts | https://docs.flutter.dev/ui/layout |
| Understanding Constraints | https://docs.flutter.dev/ui/layout/constraints |
| Container | https://api.flutter.dev/flutter/widgets/Container-class.html |
| Row | https://api.flutter.dev/flutter/widgets/Row-class.html |
| Column | https://api.flutter.dev/flutter/widgets/Column-class.html |
| Expanded | https://api.flutter.dev/flutter/widgets/Expanded-class.html |
| Flexible | https://api.flutter.dev/flutter/widgets/Flexible-class.html |
| SingleChildScrollView | https://api.flutter.dev/flutter/widgets/SingleChildScrollView-class.html |
| Adaptive and Responsive Design | https://docs.flutter.dev/ui/adaptive-responsive |

---
