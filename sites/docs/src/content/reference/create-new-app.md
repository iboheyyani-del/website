import 'dart:convert';
import 'dart:io';

import 'package:flutter/material.dart';

const String serverUrl = 'https://maher-telecom-server.onrender.com';

void main() {
  runApp(const MaherTelecomApp());
}

class MaherTelecomApp extends StatelessWidget {
  const MaherTelecomApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'ماهر للاتصالات',
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.blue,
      ),
      home: const MaherHomePage(),
    );
  }
}

class MaherHomePage extends StatefulWidget {
  const MaherHomePage({super.key});

  @override
  State<MaherHomePage> createState() => _MaherHomePageState();
}

class _MaherHomePageState extends State<MaherHomePage> {
  bool loading = true;
  bool connected = false;
  String message = 'جاري الاتصال بالخادم...';

  @override
  void initState() {
    super.initState();
    checkServer();
  }

  Future<void> checkServer() async {
    setState(() {
      loading = true;
      message = 'جاري الاتصال بالخادم...';
    });

    final client = HttpClient();

    try {
      final request = await client.getUrl(
        Uri.parse('$serverUrl/api/health'),
      );

      request.headers.set(
        HttpHeaders.acceptHeader,
        'application/json',
      );

      final response = await request.close();

      final body =
          await response.transform(utf8.decoder).join();

      final data = jsonDecode(body);

      if (!mounted) return;

      if (response.statusCode == 200 &&
          data is Map &&
          data['success'] == true) {
        setState(() {
          connected = true;
          loading = false;
          message = 'الخادم متصل ويعمل بنجاح';
        });
      } else {
        setState(() {
          connected = false;
          loading = false;
          message = 'الخادم رد ولكن توجد مشكلة';
        });
      }
    } catch (e) {
      if (!mounted) return;

      setState(() {
        connected = false;
        loading = false;
        message = 'تعذر الاتصال بالخادم';
      });
    } finally {
      client.close(force: true);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Scaffold(
        appBar: AppBar(
          centerTitle: true,
          title: const Text(
            'ماهر للاتصالات',
            style: TextStyle(
              fontWeight: FontWeight.bold,
            ),
          ),
        ),
        body: RefreshIndicator(
          onRefresh: checkServer,
          child: ListView(
            padding: const EdgeInsets.all(18),
            children: [
              Container(
                padding: const EdgeInsets.all(24),
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(24),
                  gradient: const LinearGradient(
                    colors: [
                      Colors.blue,
                      Colors.lightBlue,
                    ],
                  ),
                ),
                child: const Column(
                  crossAxisAlignment:
                      CrossAxisAlignment.end,
                  children: [
                    Text(
                      'مرحباً بك',
                      style: TextStyle(
                        color: Colors.white70,
                        fontSize: 17,
                      ),
                    ),
                    SizedBox(height: 8),
                    Text(
                      'ماهر للاتصالات',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 30,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: 10),
                    Text(
                      'هواتف • صيانة • شحن • خدمات رقمية',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 15,
                      ),
                    ),
                    SizedBox(height: 16),
                    Text(
                      '+963 932 616 631',
                      textDirection: TextDirection.ltr,
                      style: TextStyle(
                        color: Colors.white,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ],
                ),
              ),

              const SizedBox(height: 25),

              const Text(
                'حالة الخادم',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 12),

              Card(
                child: Padding(
                  padding: const EdgeInsets.all(18),
                  child: Row(
                    children: [
                      Icon(
                        connected
                            ? Icons.check_circle
                            : Icons.error,
                        color: connected
                            ? Colors.green
                            : Colors.red,
                        size: 38,
                      ),
                      const SizedBox(width: 15),
                      Expanded(
                        child: Text(
                          message,
                          style: const TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                      if (loading)
                        const SizedBox(
                          width: 22,
                          height: 22,
                          child:
                              CircularProgressIndicator(
                            strokeWidth: 2,
                          ),
                        ),
                    ],
                  ),
                ),
              ),

              const SizedBox(height: 25),

              const Text(
                'الخدمات',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 12),

              service(
                Icons.phone_android,
                'الهواتف',
                'هواتف جديدة ومستعملة',
              ),

              service(
                Icons.build,
                'صيانة الهواتف',
                'سوفتوير وصيانة وإصلاح',
              ),

              service(
                Icons.bolt,
                'شحن الألعاب والتطبيقات',
                'خدمات الشحن الرقمية',
              ),

              service(
                Icons.sim_card,
                'تعبئة الرصيد',
                'خطوط سورية وتركية',
              ),

              service(
                Icons.charging_station,
                'الشواحن والاكسسوارات',
                'جميع الأنواع والموديلات',
              ),

              service(
                Icons.tv,
                'اشتراكات الشاشة',
                'باقات وخدمات القنوات',
              ),

              const SizedBox(height: 25),

              SizedBox(
                height: 52,
                child: ElevatedButton.icon(
                  onPressed: loading ? null : checkServer,
                  icon: const Icon(Icons.refresh),
                  label: const Text(
                    'فحص الاتصال مرة أخرى',
                    style: TextStyle(fontSize: 16),
                  ),
                ),
              ),

              const SizedBox(height: 25),

              const Center(
                child: Text(
                  'خدمات ماهر للاتصالات',
                  style: TextStyle(
                    color: Colors.grey,
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget service(
    IconData icon,
    String title,
    String subtitle,
  ) {
    return Card(
      margin: const EdgeInsets.only(bottom: 10),
      child: ListTile(
        leading: CircleAvatar(
          child: Icon(icon),
        ),
        title: Text(
          title,
          style: const TextStyle(
            fontWeight: FontWeight.bold,
          ),
        ),
        subtitle: Text(subtitle),
        trailing: const Icon(
          Icons.arrow_back_ios_new,
          size: 16,
        ),
        onTap: () {},
      ),
    );
  }
}
