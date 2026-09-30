import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'ماهر للاتصالات',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('ماهر للاتصالات'),
        ),
        body: const Center(
          child: Text(
            'مرحباً بك في تطبيق ماهر للاتصالات',
            textDirection: TextDirection.rtl,
            style: TextStyle(
              fontSize: 22,
              fontWeight: FontWeight.bold,
            ),
          ),
        ),
      ),
    );
  }
}
