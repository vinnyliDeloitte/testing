# Testing Markdown

<script type="text/javascript" src="https://dartpad.dev/inject_embed.dart.js" defer></script>

```run-dartpad:theme-light:mode-dart:run-true
{$ begin main.dart $}
void main() {
  
}
{$ end main.dart $}
{$ begin solution.dart $}
void main() {
  List<int> list = [5, 20, 60, 42, 18, 90, 1];
  
  for (var i in list) {
    if (i <= 20) {
      print (i);
    }
  }
  
  print([for (var i in list) if (i <= 20) i]);
}
{$ end solution.dart $}
```

```run-dartpad:theme-light:mode-flutter:run-true
main() => print("Hello, World!");
```
