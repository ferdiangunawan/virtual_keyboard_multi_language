# virtual_keyboard_flutter

# About
A simple package for dispaying virtual keyboards on a devices like kiosks and ATMs. The library is written in Dart and has no native code dependancy.

This project has forked from `virtual_keyboard_multi_language` project because the `virtual_keyboard_multi_language` package has been discontinued and doesn't support null safety. So I refactor to nullsafety and add some usefull method to make this widget easier to use.
Feel free to PR and let's contribute.

# Feature and TODO List:
- (Done) Refactor to null safety.
- (Done) Add controller to provide editing text.
- (Done) make controller and onKeyPress callback become optional.



<br>
<p align="center">
<img style="height:200px;" alt="FlutterBlue" src="https://github.com/ferdiangunawan/virtual_keyboard_flutter/blob/master/screen3.png?raw=true" />
</p>
<br>
<br>
<p align="center">
<img style="height:200px;" alt="FlutterBlue" src="https://github.com/ferdiangunawan/virtual_keyboard_flutter/blob/master/screen2.png?raw=true" />
</p>
<hr>

# Please read:
You have to choose one between controller(`TextEditingController`) or use `onKeyPress` callback.
## Reference

### VirtualKeyboard 
Flutter widget to show virtual keyboards.
```dart
// Keyboard Type: Can be Numeric or Alphanumeric.
VirtualKeyboardType type
```
```dart
// Callback for Key press event. Called with pressed `Key` object.
Function onKeyPress;
```
```dart
// Virtual keyboard height. Default is 300.
double height;
```
```dart
/// Virtual keyboard height. Default is full screen width
  double width;
```
```dart
// Color for key texts and icons.
Color textColor;
```
```dart
// Font size for keyboard keys.
double fontSize;;
```
```dart
// Only Caps letters enabled.
bool alwaysCaps;;
```
```dart
/// the custom layout for multi or single language
VirtualKeyboardLayoutKeys customLayoutKeys;
```
```dart
/// used for multi-languages with default layouts, the default is English only
/// will be ignored if customLayoutKeys is not null
List<VirtualKeyboardDefaultLayouts> defaultLayouts;
```
```dart
/// inverse the layout to fix the issues with right to left languages, default is false.
bool reverseLayout;
```

### VirtualKeyboardType
enum of Available Virtual Keyboard Types.
```dart
// Numeric only.
VirtualKeyboardType.Numeric
```
```dart
// Alphanumeric: letters`[A-Z]` + numbers`[0-9]` + `@` + `.`.
VirtualKeyboardType.Alphanumeric
```

### VirtualKeyboardKey
Virtual Keyboard key.
```dart
// The text of the key. 
String text
```
```dart
// The capitalized text of the key. 
String capsText;
```
```dart
// Action or String
VirtualKeyboardKeyType keyType;
```
```dart
// Action of the key.
VirtualKeyboardKeyAction action;
```
### VirtualKeyboardKeyType
Type for virtual keyboard key.

```dart
// Can be an action key - Return, Backspace, etc.
VirtualKeyboardKeyType.Action
```
```dart
// Keys that have text values - letters, numbers, comma ...
VirtualKeyboardKeyType.String
```

### VirtualKeyboardKeyAction
```dart
/// Virtual keyboard actions.
enum VirtualKeyboardKeyAction { Backspace, Return, Shift, Space }
```

## Usage

#### Show Alphanumeric keyboard with default view
```dart
// Wrap the keyboard with Container to set background color.
Container(
            // Keyboard is transparent
            color: Colors.deepPurple,
            child: VirtualKeyboard(
                // Default height is 300
                height: 350,
                // Default height is will screen width
                width: 600,
                // Default is black
                textColor: Colors.white,
                // Default 14
                fontSize: 20,
                // the layouts supported
                defaultLayouts = [VirtualKeyboardDefaultLayouts.English],
                // [A-Z, 0-9]
                type: VirtualKeyboardType.Alphanumeric,
                // Callback for key press event
                onKeyPress: _onKeyPress),
          )
```

#### Show Numeric keyboard with default view
```dart
Container(
            // Keyboard is transparent
            color: Colors.red,
            child: VirtualKeyboard(
                // [0-9] + .
                type: VirtualKeyboardType.Numeric,
                // Callback for key press event
                onKeyPress: (key) => print(key.text)),
          )
```

#### Show Alphanumeric keyboard with customized keys

```dart
 Container(
            color: Colors.deepPurple,
            child: VirtualKeyboard(
                height: keyboardHeight,
                textColor: Colors.white,
                fontSize: 20,
                builder: _builder,
                type: VirtualKeyboardType.Numeric,
                onKeyPress: _onKeyPress),
          )

  /// Builder for keyboard keys.
  Widget _builder(BuildContext context, VirtualKeyboardKey key) {
    Widget keyWidget;

    switch (key.keyType) {
      case VirtualKeyboardKeyType.String:
        // Draw String key.
        keyWidget = _keyboardDefaultKey(key);
        break;
      case VirtualKeyboardKeyType.Action:
        // Draw action key.
        keyWidget = _keyboardDefaultActionKey(key);
        break;
    }

    return keyWidget;
  }          
```

#### onKeyPressed event basic ussage example
```dart
// Just local variable. Use TextEditingController or similar to show in UI.
TextEditingController _controller = TextEditingController();

  /// Fired when the virtual keyboard key is pressed.
_onKeyPress(VirtualKeyboardKey key) {
if (key.keyType == VirtualKeyboardKeyType.String) {
    _controller.text += (shiftEnabled ? key.capsText : key.text);
} else if (key.keyType == VirtualKeyboardKeyType.Action) {
    switch (key.action) {
    case VirtualKeyboardKeyAction.Backspace:
        if (_controller.text.length == 0) return;
        _controller.text = _controller.text.substring(0, _controller.text.length - 1);
        break;
    case VirtualKeyboardKeyAction.Return:
        _controller.text = _controller.text + '\n';
        break;
    case VirtualKeyboardKeyAction.Space:
        _controller.text = _controller.text + key.text;
        break;
    case VirtualKeyboardKeyAction.Shift:
        shiftEnabled = !shiftEnabled;
        break;
    default:
    }
}
// Update the screen
setState(() {});
}
```

- [Ferdian Gunawan](https://github.com/ferdiangunawan)
- [LICENSE - MIT](https://github.com/digitalpomegranate/virtual_keyboard/blob/master/LICENSE)

