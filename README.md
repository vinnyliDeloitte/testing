# Testing

<script type="text/javascript" src="https://dartpad.dev/inject_embed.dart.js" defer></script>

```run-dartpad:theme-dark:mode-flutter:run-true:width-80%:height-800px
{$ begin main.dart $}
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

final stringProvider = StateProvider<String>((ref) => 'Hello world');

void main() => runApp(ProviderScope(child: MyApp()));

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Riverpod Activity',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends ConsumerWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Page'),
      ),
      body: Center(
        child: Text(
          '',
        ),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

final stringProvider = StateProvider<String>((ref) => 'Hello world');

void main() => runApp(ProviderScope(child: MyApp()));

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Riverpod Activity',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends ConsumerWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    TextEditingController stringController = TextEditingController();
    final string = ref.watch(stringProvider);
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Page'),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Center(
            child: Text(
              string,
            ),
          ),
          TextField(
            decoration: const InputDecoration(labelText: 'Enter Text Here'),
            controller: stringController,
          ),
          TextButton(
            child: const Text('Submit'),
            onPressed: () {
              ref
                  .read(stringProvider.notifier)
                  .update((state) => stringController.text);
              stringController.clear();
            },
          ),
        ],
      ),
    );
  }
}
{$ end solution.dart $}
```
