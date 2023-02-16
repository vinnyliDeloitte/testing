# Testing

<script type="text/javascript" src="https://dartpad.dev/inject_embed.dart.js" defer></script>

```run-dartpad:theme-dark:mode-flutter:run-true:width-90%:height-800px
{$ begin main.dart $}
// ignore_for_file: depend_on_referenced_packages
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() => runApp(const MyApp());

// DATA LAYER
// Data Sources
abstract class WeatherDataSource {
  Future<http.Response> getWeatherForLocation({
    required String zip,
  });
}

class WeatherDataSourceImpl implements WeatherDataSource {
  @override
  Future<http.Response> getWeatherForLocation({
    required String zip,
  }) async {
    final url = Uri.https('api.openweathermap.org', '/data/2.5/weather',
        {'zip': zip, 'appid': 'edc1eb51a6b19e7d90eefd151f3b8070'});
    final response = await http.get(url);
    return response;
  }
}

//DOMAIN LAYER

// PRESENTATION LAYER
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  final String title;

  const MyHomePage({
    Key? key,
    required this.title,
  }) : super(key: key);

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
// ignore_for_file: depend_on_referenced_packages, avoid_print, prefer_interpolation_to_compose_strings
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'package:flutter_riverpod/flutter_riverpod.dart';

// Main App
void main() => runApp(const ProviderScope(child: MyApp()));

final stringProvider =
    StateProvider<String>((ref) => 'Click on Get Random Joke to get a Joke');

// DATA LAYER
// Data Sources
abstract class JokeRemoteDataSource {
  Future<http.Response> getRandomJoke();
}

class JokeRemoteDataSourceImpl implements JokeRemoteDataSource {
  @override
  Future<http.Response> getRandomJoke() async {
    final url =
        Uri.https('official-joke-api.appspot.com', '/jokes/programming/random');
    // Await the HTTP GET response, then decode the
    // JSON data it contains.
    final response = await http.get(url);
    return response;
  }
}

// Mapper
class JokeMapper {
  static Joke fromJson(Map<String, dynamic> json) {
    return Joke(
      type: json['type'],
      setup: json['setup'],
      punchline: json['punchline'],
    );
  }
}

// DOMAIN LAYER
class Joke {
  final String type;
  final String setup;
  final String punchline;

  const Joke({
    required this.type,
    required this.setup,
    required this.punchline,
  });
}

// Repository
// This is how we would write the repository, we will not be using this for this dartpad activity

// PRESENTATION LAYER
class JokeLandingViewProvider {
  static final jokeViewController = Provider((ref) {
    return JokeLandingViewController();
  });
}

class JokeLandingViewController {
  void onNewJoke(WidgetRef ref) async {
    JokeRemoteDataSourceImpl().getRandomJoke().then((value) {
      var decode = jsonDecode(value.body) as List;
      final joke = JokeMapper.fromJson(decode[0]);
      ref
          .read(stringProvider.notifier)
          .update((state) => joke.setup + "\n\n" + joke.punchline);
    });
  }
}

// Main Widget presentation
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'USDC Mobile Activity',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends ConsumerWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final jokeString = ref.watch(stringProvider);
    return Scaffold(
      appBar: AppBar(
        title: const Text('Arichitecture Activity'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              jokeString,
              textAlign: TextAlign.center,
            ),
          ],
        ),
      ),
      floatingActionButton: TextButton(
        style: TextButton.styleFrom(
            foregroundColor: Colors.white, backgroundColor: Colors.blue),
        onPressed: (() => ref
            .read(JokeLandingViewProvider.jokeViewController)
            .onNewJoke(ref)),
        child: const Text('Get Random Joke'),
      ),
    );
  }
}
{$ end solution.dart $}
```
