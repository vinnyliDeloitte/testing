# Testing Markdown

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
