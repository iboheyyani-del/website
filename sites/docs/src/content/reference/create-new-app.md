import 'package:flutter/material.dart';

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
        fontFamily: 'Arial',
      ),
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  int currentIndex = 0;

  final pages = const [
    HomePage(),
    ServicesPage(),
    SettingsPage(),
  ];

  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Scaffold(
        body: SafeArea(
          child: pages[currentIndex],
        ),

        // شريط التحكم السفلي
        bottomNavigationBar: NavigationBar(
          selectedIndex: currentIndex,
          onDestinationSelected: (index) {
            setState(() {
              currentIndex = index;
            });
          },
          destinations: const [
            NavigationDestination(
              icon: Icon(Icons.home_outlined),
              selectedIcon: Icon(Icons.home),
              label: 'الرئيسية',
            ),
            NavigationDestination(
              icon: Icon(Icons.flash_on_outlined),
              selectedIcon: Icon(Icons.flash_on),
              label: 'الخدمات',
            ),
            NavigationDestination(
              icon: Icon(Icons.settings_outlined),
              selectedIcon: Icon(Icons.settings),
              label: 'الإعدادات',
            ),
          ],
        ),

        // مساعد ماهر
        floatingActionButton: FloatingActionButton.extended(
          onPressed: () {
            showModalBottomSheet(
              context: context,
              showDragHandle: true,
              builder: (context) {
                return const AssistantPage();
              },
            );
          },
          icon: const Icon(Icons.smart_toy),
          label: const Text('مساعد ماهر'),
        ),
      ),
    );
  }
}

// =====================================================
// الصفحة الرئيسية
// =====================================================

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return CustomScrollView(
      slivers: [

        // العنوان
        SliverToBoxAdapter(
          child: Padding(
            padding: const EdgeInsets.all(18),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [

                const Text(
                  'ماهر للاتصالات',
                  style: TextStyle(
                    fontSize: 28,
                    fontWeight: FontWeight.bold,
                  ),
                ),

                const SizedBox(height: 5),

                const Text(
                  'متجر الأجهزة والخدمات الرقمية',
                  style: TextStyle(
                    color: Colors.grey,
                  ),
                ),

                const SizedBox(height: 18),

                // البحث
                TextField(
                  decoration: InputDecoration(
                    hintText: 'ابحث عن هاتف أو شاحن أو سماعة...',
                    prefixIcon: const Icon(Icons.search),
                    filled: true,
                    fillColor: Colors.white,
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(18),
                      borderSide: BorderSide.none,
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),

        // الأقسام
        SliverToBoxAdapter(
          child: SizedBox(
            height: 115,
            child: ListView(
              scrollDirection: Axis.horizontal,
              padding: const EdgeInsets.symmetric(horizontal: 15),
              children: const [

                CategoryItem(
                  icon: Icons.smartphone,
                  title: 'هواتف جديدة',
                ),

                CategoryItem(
                  icon: Icons.phone_android,
                  title: 'مستعمل',
                ),

                CategoryItem(
                  icon: Icons.bolt,
                  title: 'شواحن',
                ),

                CategoryItem(
                  icon: Icons.headphones,
                  title: 'سماعات',
                ),

                CategoryItem(
                  icon: Icons.speaker,
                  title: 'سبيكرات',
                ),

                CategoryItem(
                  icon: Icons.shield,
                  title: 'حماية',
                ),

                CategoryItem(
                  icon: Icons.build,
                  title: 'الصيانة',
                ),
              ],
            ),
          ),
        ),

        // الهواتف الجديدة
        const SectionTitle(
          title: '📱 الهواتف الجديدة',
        ),

        const ProductGrid(
          products: [
            Product(
              name: 'Samsung Galaxy',
              price: 'السعر',
              description: 'أضف الصور والمواصفات من لوحة التحكم',
            ),
            Product(
              name: 'iPhone',
              price: 'السعر',
              description: 'أضف الصور والمواصفات من لوحة التحكم',
            ),
          ],
        ),

        // المستعمل
        const SectionTitle(
          title: '📱 الهواتف المستعملة',
        ),

        const ProductGrid(
          products: [
            Product(
              name: 'هاتف مستعمل',
              price: 'السعر',
              description: 'الحالة والصور والمواصفات',
            ),
            Product(
              name: 'هاتف مستعمل آخر',
              price: 'السعر',
              description: 'الحالة والصور والمواصفات',
            ),
          ],
        ),

        // الشواحن والسماعات
        const SectionTitle(
          title: '🔌 الشواحن والسماعات',
        ),

        const ProductGrid(
          products: [
            Product(
              name: 'شاحن Type-C',
              price: 'السعر',
              description: 'صور ومواصفات المنتج',
            ),
            Product(
              name: 'سماعة',
              price: 'السعر',
              description: 'صور ومواصفات المنتج',
            ),
            Product(
              name: 'سبيكر صوتي',
              price: 'السعر',
              description: 'صور ومواصفات المنتج',
            ),
          ],
        ),

        const SliverToBoxAdapter(
          child: SizedBox(height: 100),
        ),
      ],
    );
  }
}

// =====================================================
// قسم الخدمات
// =====================================================

class ServicesPage extends StatelessWidget {
  const ServicesPage({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView(
      padding: const EdgeInsets.all(18),
      children: [

        const Text(
          'الخدمات الرقمية',
          style: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.bold,
          ),
        ),

        const SizedBox(height: 20),

        ServiceItem(
          icon: Icons.phone_android,
          title: 'تعبئة الرصيد',
        ),

        ServiceItem(
          icon: Icons.sports_esports,
          title: 'شحن الألعاب',
        ),

        ServiceItem(
          icon: Icons.apps,
          title: 'شحن التطبيقات',
        ),

        ServiceItem(
          icon: Icons.tv,
          title: 'اشتراكات قنوات الشاشة',
        ),

        ServiceItem(
          icon: Icons.build,
          title: 'خدمات الصيانة',
        ),
      ],
    );
  }
}

// =====================================================
// الإعدادات
// =====================================================

class SettingsPage extends StatelessWidget {
  const SettingsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView(
      padding: const EdgeInsets.all(18),
      children: const [

        Text(
          'الإعدادات',
          style: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.bold,
          ),
        ),

        SizedBox(height: 20),

        ListTile(
          leading: Icon(Icons.person),
          title: Text('حسابي'),
        ),

        ListTile(
          leading: Icon(Icons.receipt_long),
          title: Text('طلباتي'),
        ),

        ListTile(
          leading: Icon(Icons.notifications),
          title: Text('الإشعارات'),
        ),

        ListTile(
          leading: Icon(Icons.language),
          title: Text('اللغة'),
        ),

        ListTile(
          leading: Icon(Icons.currency_exchange),
          title: Text('العملة'),
        ),

        ListTile(
          leading: Icon(Icons.support_agent),
          title: Text('الدعم والتواصل'),
        ),

        ListTile(
          leading: Icon(Icons.info),
          title: Text('حول التطبيق'),
        ),
      ],
    );
  }
}

// =====================================================
// مساعد ماهر
// =====================================================

class AssistantPage extends StatelessWidget {
  const AssistantPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [

            const Text(
              '🤖 مساعد ماهر',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 10),

            const Text(
              'اسألني عن المنتجات أو الخدمات أو طلباتك.',
            ),

            const SizedBox(height: 20),

            TextField(
              decoration: InputDecoration(
                hintText: 'اكتب سؤالك...',
                suffixIcon: IconButton(
                  onPressed: () {},
                  icon: const Icon(Icons.send),
                ),
                border: const OutlineInputBorder(),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// =====================================================
// عناصر الواجهة
// =====================================================

class CategoryItem extends StatelessWidget {
  final IconData icon;
  final String title;

  const CategoryItem({
    super.key,
    required this.icon,
    required this.title,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 100,
      margin: const EdgeInsets.only(left: 10),
      child: Card(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(icon, size: 30),
            const SizedBox(height: 8),
            Text(
              title,
              textAlign: TextAlign.center,
            ),
          ],
        ),
      ),
    );
  }
}

class SectionTitle extends StatelessWidget {
  final String title;

  const SectionTitle({
    super.key,
    required this.title,
  });

  @override
  Widget build(BuildContext context) {
    return SliverToBoxAdapter(
      child: Padding(
        padding: const EdgeInsets.fromLTRB(18, 18, 18, 10),
        child: Text(
          title,
          style: const TextStyle(
            fontSize: 21,
            fontWeight: FontWeight.bold,
          ),
        ),
      ),
    );
  }
}

class Product {
  final String name;
 
