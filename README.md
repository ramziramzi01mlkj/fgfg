import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:google_fonts/google_fonts.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  await SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp,
    DeviceOrientation.portraitDown,
  ]);
  
  runApp(const OfflineHubApp());
}

// ==================== COLORS ====================
class AppColors {
  static const Color primary = Color(0xFF6C63FF);
  static const Color secondary = Color(0xFF5B54E8);
  static const Color background = Color(0xFF0A0E21);
  static const Color cardDark = Color(0xFF1D1E33);
  static const Color textGrey = Color(0xFF8D8E98);
  static const Color success = Color(0xFF4CAF50);
  static const Color error = Color(0xFFE74C3C);
  
  static const LinearGradient primaryGradient = LinearGradient(
    colors: [primary, secondary],
    begin: Alignment.topLeft,
    end: Alignment.bottomRight,
  );
}

// ==================== MAIN APP ====================
class OfflineHubApp extends StatelessWidget {
  const OfflineHubApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'OfflineHub',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        brightness: Brightness.dark,
        primaryColor: AppColors.primary,
        scaffoldBackgroundColor: AppColors.background,
        colorScheme: const ColorScheme.dark(
          primary: AppColors.primary,
          secondary: AppColors.secondary,
        ),
      ),
      home: const SplashScreen(),
    );
  }
}

// ==================== SPLASH SCREEN ====================
class SplashScreen extends StatefulWidget {
  const SplashScreen({super.key});

  @override
  State<SplashScreen> createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen> 
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _fadeAnimation;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 2),
    );

    _fadeAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeIn),
    );

    _scaleAnimation = Tween<double>(begin: 0.5, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.elasticOut),
    );

    _controller.forward();

    Future.delayed(const Duration(seconds: 3), () {
      if (mounted) {
        Navigator.of(context).pushReplacement(
          MaterialPageRoute(builder: (_) => const LoginScreen()),
        );
      }
    });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: AppColors.primaryGradient,
        ),
        child: Center(
          child: FadeTransition(
            opacity: _fadeAnimation,
            child: ScaleTransition(
              scale: _scaleAnimation,
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Container(
                    width: 150,
                    height: 150,
                    decoration: BoxDecoration(
                      color: Colors.white,
                      borderRadius: BorderRadius.circular(40),
                      boxShadow: [
                        BoxShadow(
                          color: Colors.black.withOpacity(0.3),
                          blurRadius: 30,
                          offset: const Offset(0, 15),
                        ),
                      ],
                    ),
                    child: const Icon(
                      Icons.cloud_download_rounded,
                      size: 80,
                      color: AppColors.primary,
                    ),
                  ),
                  const SizedBox(height: 40),
                  Text(
                    'OfflineHub',
                    style: GoogleFonts.cairo(
                      fontSize: 56,
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                    ),
                  ),
                  const SizedBox(height: 10),
                  Text(
                    'ابقَ متصلاً حتى بدون إنترنت',
                    style: GoogleFonts.cairo(
                      fontSize: 18,
                      color: Colors.white.withOpacity(0.9),
                    ),
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

// ==================== LOGIN SCREEN ====================
class LoginScreen extends StatelessWidget {
  const LoginScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: AppColors.primaryGradient,
        ),
        child: SafeArea(
          child: Center(
            child: Padding(
              padding: const EdgeInsets.symmetric(horizontal: 30),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Container(
                    width: 120,
                    height: 120,
                    decoration: BoxDecoration(
                      color: Colors.white,
                      borderRadius: BorderRadius.circular(30),
                      boxShadow: [
                        BoxShadow(
                          color: Colors.black.withOpacity(0.2),
                          blurRadius: 20,
                          offset: const Offset(0, 10),
                        ),
                      ],
                    ),
                    child: const Icon(
                      Icons.cloud_download_rounded,
                      size: 60,
                      color: AppColors.primary,
                    ),
                  ),
                  
                  const SizedBox(height: 40),
                  
                  Text(
                    'OfflineHub',
                    style: GoogleFonts.cairo(
                      fontSize: 48,
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                    ),
                  ),
                  
                  const SizedBox(height: 10),
                  
                  Text(
                    'ابقَ متصلاً حتى بدون إنترنت',
                    style: GoogleFonts.cairo(
                      fontSize: 18,
                      color: Colors.white.withOpacity(0.9),
                    ),
                    textAlign: TextAlign.center,
                  ),
                  
                  const SizedBox(height: 60),
                  
                  ElevatedButton(
                    onPressed: () {
                      Navigator.of(context).pushReplacement(
                        MaterialPageRoute(builder: (_) => const HomeScreen()),
                      );
                    },
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.white,
                      padding: const EdgeInsets.symmetric(
                        horizontal: 40,
                        vertical: 15,
                      ),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(30),
                      ),
                      elevation: 5,
                    ),
                    child: Row(
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        const Icon(Icons.login, color: AppColors.primary),
                        const SizedBox(width: 10),
                        Text(
                          'الدخول إلى التطبيق',
                          style: GoogleFonts.cairo(
                            fontSize: 16,
                            fontWeight: FontWeight.w600,
                            color: AppColors.primary,
                          ),
                        ),
                      ],
                    ),
                  ),
                  
                  const SizedBox(height: 40),
                  
                  _buildFeature(Icons.video_library, 'ريلز بدون إنترنت'),
                  const SizedBox(height: 15),
                  _buildFeature(Icons.message, 'رسائل عبر Bluetooth'),
                  const SizedBox(height: 15),
                  _buildFeature(Icons.map, 'خرائط Offline'),
                  const SizedBox(height: 15),
                  _buildFeature(Icons.article, 'مقالات وأخبار'),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildFeature(IconData icon, String text) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(icon, color: Colors.white, size: 20),
        const SizedBox(width: 10),
        Text(
          text,
          style: GoogleFonts.cairo(
            color: Colors.white.withOpacity(0.8),
            fontSize: 14,
          ),
        ),
      ],
    );
  }
}

// ==================== HOME SCREEN ====================
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> 
    with SingleTickerProviderStateMixin {
  late TabController _tabController;
  int _selectedIndex = 0;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 2, vsync: this);
    _tabController.addListener(() {
      setState(() => _selectedIndex = _tabController.index);
    });
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      appBar: AppBar(
        backgroundColor: AppColors.cardDark,
        elevation: 0,
        title: Text(
          'OfflineHub',
          style: GoogleFonts.cairo(
            fontWeight: FontWeight.bold,
            fontSize: 22,
          ),
        ),
        centerTitle: true,
        actions: [
          IconButton(
            icon: const Icon(Icons.settings),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(builder: (_) => const SettingsScreen()),
              );
            },
          ),
        ],
        bottom: TabBar(
          controller: _tabController,
          indicatorColor: AppColors.primary,
          labelColor: AppColors.primary,
          unselectedLabelColor: AppColors.textGrey,
          labelStyle: GoogleFonts.cairo(
            fontSize: 16,
            fontWeight: FontWeight.w600,
          ),
          tabs: const [
            Tab(icon: Icon(Icons.play_circle_outline), text: 'المحتوى'),
            Tab(icon: Icon(Icons.connect_without_contact), text: 'الاتصال'),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: const [
          ContentTab(),
          ConnectTab(),
        ],
      ),
    );
  }
}

// ==================== CONTENT TAB ====================
class ContentTab extends StatelessWidget {
  const ContentTab({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView(
      padding: const EdgeInsets.all(15),
      children: [
        const SizedBox(height: 10),
        
        _buildSectionCard(
          context,
          icon: Icons.play_circle_filled,
          title: 'ريلز',
          subtitle: '20 فيديو جديد متاح',
          color: Colors.red,
          onTap: () => Navigator.push(
            context,
            MaterialPageRoute(builder: (_) => const ReelsScreen()),
          ),
        ),
        
        const SizedBox(height: 15),
        
        _buildSectionCard(
          context,
          icon: Icons.article,
          title: 'مقالات',
          subtitle: '15 مقال جديد',
          color: Colors.blue,
          onTap: () => Navigator.push(
            context,
            MaterialPageRoute(builder: (_) => const ArticlesScreen()),
          ),
        ),
        
        const SizedBox(height: 15),
        
        _buildSectionCard(
          context,
          icon: Icons.newspaper,
          title: 'أخبار',
          subtitle: 'آخر الأخبار',
          color: Colors.orange,
          onTap: () {},
        ),
        
        const SizedBox(height: 20),
        
        Container(
          padding: const EdgeInsets.all(20),
          decoration: BoxDecoration(
            gradient: AppColors.primaryGradient,
            borderRadius: BorderRadius.circular(15),
          ),
          child: Column(
            children: [
              const Icon(Icons.cloud_download, color: Colors.white, size: 40),
              const SizedBox(height: 10),
              Text(
                'المحتوى المتاح Offline',
                style: GoogleFonts.cairo(
                  color: Colors.white,
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                ),
              ),
              const SizedBox(height: 5),
              Text(
                '35 عنصر محفوظ (2.5 GB)',
                style: GoogleFonts.cairo(
                  color: Colors.white.withOpacity(0.9),
                  fontSize: 14,
                ),
              ),
            ],
          ),
        ),
      ],
    );
  }

  Widget _buildSectionCard(
    BuildContext context, {
    required IconData icon,
    required String title,
    required String subtitle,
    required Color color,
    required VoidCallback onTap,
  }) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        padding: const EdgeInsets.all(20),
        decoration: BoxDecoration(
          color: AppColors.cardDark,
          borderRadius: BorderRadius.circular(15),
          boxShadow: [
            BoxShadow(
              color: Colors.black.withOpacity(0.2),
              blurRadius: 10,
              offset: const Offset(0, 5),
            ),
          ],
        ),
        child: Row(
          children: [
            Container(
              padding: const EdgeInsets.all(15),
              decoration: BoxDecoration(
                color: color.withOpacity(0.2),
                borderRadius: BorderRadius.circular(12),
              ),
              child: Icon(icon, color: color, size: 30),
            ),
            const SizedBox(width: 20),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    title,
                    style: GoogleFonts.cairo(
                      color: Colors.white,
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 5),
                  Text(
                    subtitle,
                    style: GoogleFonts.cairo(
                      color: AppColors.textGrey,
                      fontSize: 14,
                    ),
                  ),
                ],
              ),
            ),
            const Icon(Icons.arrow_forward_ios, color: AppColors.textGrey),
          ],
        ),
      ),
    );
  }
}

// ==================== CONNECT TAB ====================
class ConnectTab extends StatelessWidget {
  const ConnectTab({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView(
      padding: const EdgeInsets.all(15),
      children: [
        const SizedBox(height: 10),
        
        _buildSectionCard(
          context,
          icon: Icons.message,
          title: 'الرسائل',
          subtitle: 'رسائل عبر Bluetooth',
          badge: '3',
          onTap: () => Navigator.push(
            context,
            MaterialPageRoute(builder: (_) => const MessagesScreen()),
          ),
        ),
        
        const SizedBox(height: 15),
        
        _buildSectionCard(
          context,
          icon: Icons.map,
          title: 'الخرائط',
          subtitle: 'تصفح الخرائط بدون نت',
          onTap: () => Navigator.push(
            context,
            MaterialPageRoute(builder: (_) => const MapsScreen()),
          ),
        ),
        
        const SizedBox(height: 20),
        
        Container(
          padding: const EdgeInsets.all(20),
          decoration: BoxDecoration(
            color: AppColors.cardDark,
            borderRadius: BorderRadius.circular(15),
          ),
          child: Row(
            children: [
              const Icon(Icons.bluetooth_connected, color: AppColors.primary),
              const SizedBox(width: 15),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Bluetooth نشط',
                      style: GoogleFonts.cairo(
                        color: Colors.white,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 5),
                    Text(
                      '5 أجهزة قريبة',
                      style: GoogleFonts.cairo(
                        color: AppColors.textGrey,
                        fontSize: 12,
                      ),
                    ),
                  ],
                ),
              ),
              Switch(
                value: true,
                onChanged: (val) {},
                activeColor: AppColors.primary,
              ),
            ],
          ),
        ),
      ],
    );
  }

  Widget _buildSectionCard(
    BuildContext context, {
    required IconData icon,
    required String title,
    required String subtitle,
    String? badge,
    required VoidCallback onTap,
  }) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        padding: const EdgeInsets.all(20),
        decoration: BoxDecoration(
          color: AppColors.cardDark,
          borderRadius: BorderRadius.circular(15),
          boxShadow: [
            BoxShadow(
              color: Colors.black.withOpacity(0.2),
              blurRadius: 10,
              offset: const Offset(0, 5),
            ),
          ],
        ),
        child: Row(
          children: [
            Stack(
              children: [
                Container(
                  padding: const EdgeInsets.all(15),
                  decoration: BoxDecoration(
                    gradient: AppColors.primaryGradient,
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Icon(icon, color: Colors.white, size: 30),
                ),
                if (badge != null)
                  Positioned(
                    right: 0,
                    top: 0,
                    child: Container(
                      padding: const EdgeInsets.all(5),
                      decoration: const BoxDecoration(
                        color: Colors.red,
                        shape: BoxShape.circle,
                      ),
                      child: Text(
                        badge,
                        style: const TextStyle(
                          color: Colors.white,
                          fontSize: 10,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),
              ],
            ),
            const SizedBox(width: 20),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    title,
                    style: GoogleFonts.cairo(
                      color: Colors.white,
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 5),
                  Text(
                    subtitle,
                    style: GoogleFonts.cairo(
                      color: AppColors.textGrey,
                      fontSize: 14,
                    ),
                  ),
                ],
              ),
            ),
            const Icon(Icons.arrow_forward_ios, color: AppColors.textGrey),
          ],
        ),
      ),
    );
  }
}

// ==================== REELS SCREEN ====================
class ReelsScreen extends StatelessWidget {
  const ReelsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      extendBodyBehindAppBar: true,
      appBar: AppBar(
        backgroundColor: Colors.transparent,
        elevation: 0,
      ),
      body: PageView.builder(
        scrollDirection: Axis.vertical,
        itemCount: 5,
        itemBuilder: (context, index) {
          return Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                begin: Alignment.topCenter,
                end: Alignment.bottomCenter,
                colors: [
                  Colors.primaries[index % Colors.primaries.length].withOpacity(0.6),
                  Colors.black,
                ],
              ),
            ),
            child: Stack(
              children: [
                Center(
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      const Icon(
                        Icons.play_circle_outline,
                        size: 100,
                        color: Colors.white,
                      ),
                      const SizedBox(height: 20),
                      Text(
                        'ريل رقم ${index + 1}',
                        style: GoogleFonts.cairo(
                          fontSize: 32,
                          fontWeight: FontWeight.bold,
                          color: Colors.white,
                        ),
                      ),
                    ],
                  ),
                ),
                
                Positioned(
                  right: 15,
                  bottom: 100,
                  child: Column(
                    children: [
                      _buildAction(Icons.favorite, '1.2K'),
                      const SizedBox(height: 25),
                      _buildAction(Icons.comment, '234'),
                      const SizedBox(height: 25),
                      _buildAction(Icons.share, 'مشاركة'),
                      const SizedBox(height: 25),
                      _buildAction(Icons.offline_pin, 'محفوظ'),
                    ],
                  ),
                ),
                
                Positioned(
                  left: 15,
                  right: 80,
                  bottom: 80,
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        children: [
                          const CircleAvatar(
                            backgroundColor: AppColors.primary,
                            child: Icon(Icons.person, color: Colors.white),
                          ),
                          const SizedBox(width: 10),
                          Text(
                            'مستخدم ${index + 1}',
                            style: GoogleFonts.cairo(
                              color: Colors.white,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 10),
                      Text(
                        'محتوى رائع ومميز 🎬',
                        style: GoogleFonts.cairo(
                          color: Colors.white,
                          fontSize: 16,
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
          );
        },
      ),
    );
  }

  Widget _buildAction(IconData icon, String label) {
    return Column(
      children: [
        Icon(icon, color: Colors.white, size: 32),
        const SizedBox(height: 5),
        Text(
          label,
          style: GoogleFonts.cairo(
            color: Colors.white,
            fontSize: 12,
            fontWeight: FontWeight.w600,
          ),
        ),
      ],
    );
  }
}

// ==================== ARTICLES SCREEN ====================
class ArticlesScreen extends StatelessWidget {
  const ArticlesScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      appBar: AppBar(
        backgroundColor: AppColors.cardDark,
        title: Text('المقالات', style: GoogleFonts.cairo()),
      ),
      body: ListView.builder(
        padding: const EdgeInsets.all(15),
        itemCount: 10,
        itemBuilder: (context, index) {
          return Container(
            margin: const EdgeInsets.only(bottom: 15),
            decoration: BoxDecoration(
              color: AppColors.cardDark,
              borderRadius: BorderRadius.circular(15),
            ),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Container(
                  height: 180,
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      colors: [
                        Colors.primaries[index % Colors.primaries.length],
                        Colors.primaries[index % Colors.primaries.length].withOpacity(0.6),
                      ],
                    ),
                    borderRadius: const BorderRadius.vertical(
                      top: Radius.circular(15),
                    ),
                  ),
                  child: const Center(
                    child: Icon(Icons.image, size: 60, color: Colors.white),
                  ),
                ),
                Padding(
                  padding: const EdgeInsets.all(15),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Container(
                        padding: const EdgeInsets.symmetric(
                          horizontal: 12,
                          vertical: 6,
                        ),
                        decoration: BoxDecoration(
                          color: AppColors.primary.withOpacity(0.2),
                          borderRadius: BorderRadius.circular(20),
                        ),
                        child: Text(
                          'تقنية',
                          style: GoogleFonts.cairo(
                            color: AppColors.primary,
                            fontSize: 12,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                      const SizedBox(height: 10),
                      Text(
                        'مقالة رقم ${index + 1}: عنوان مميز',
                        style: GoogleFonts.cairo(
                          color: Colors.white,
                          fontSize: 18,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Text(
                        'نص تجريبي للمقالة. محتوى رائع ومفيد...',
                        style: GoogleFonts.cairo(
                          color: AppColors.textGrey,
                          fontSize: 14,
                        ),
                        maxLines: 2,
                      ),
                      const SizedBox(height: 10),
                      Row(
                        children: [
                          const Icon(Icons.person, size: 16, color: AppColors.textGrey),
                          const SizedBox(width: 5),
                          Text(
                            'الكاتب',
                            style: GoogleFonts.cairo(
                              color: AppColors.textGrey,
                              fontSize: 12,
                            ),
                          ),
                          const Spacer(),
                          const Icon(Icons.offline_pin, size: 16, color: AppColors.success),
                        ],
                      ),
                    ],
                  ),
                ),
              ],
            ),
          );
        },
      ),
    );
  }
}

// ==================== MESSAGES SCREEN ====================
class MessagesScreen extends StatefulWidget {
  const MessagesScreen({super.key});

  @override
  State<MessagesScreen> createState() => _MessagesScreenState();
}

class _MessagesScreenState extends State<MessagesScreen> {
  final List<Map<String, dynamic>> _messages = [];
  final TextEditingController _controller = TextEditingController();

  void _sendMessage() {
    if (_controller.text.trim().isEmpty) return;
    
    setState(() {
      _messages.add({
        'text': _controller.text,
        'isMe': true,
        'time': DateTime.now(),
      });
      _controller.clear();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      appBar: AppBar(
        backgroundColor: AppColors.cardDark,
        title: Row(
          children: [
            const CircleAvatar(
              radius: 18,
              backgroundColor: AppColors.primary,
              child: Icon(Icons.person, size: 20),
            ),
            const SizedBox(width: 10),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text('مستخدم', style: GoogleFonts.cairo(fontSize: 16)),
                Text(
                  'عبر Bluetooth',
                  style: GoogleFonts.cairo(
                    fontSize: 11,
                    color: AppColors.success,
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
      body: Column(
        children: [
          Expanded(
            child: _messages.isEmpty
                ? Center(
                    child: Text(
                      'ابدأ المحادثة',
                      style: GoogleFonts.cairo(
                        color: AppColors.textGrey,
                        fontSize: 16,
                      ),
                    ),
                  )
                : ListView.builder(
                    padding: const EdgeInsets.all(15),
                    itemCount: _messages.length,
                    itemBuilder: (context, index) {
                      final msg = _messages[index];
                      return Align(
                        alignment: msg['isMe']
                            ? Alignment.centerRight
                            : Alignment.centerLeft,
                        child: Container(
                          margin: const EdgeInsets.only(bottom: 10),
                          padding: const EdgeInsets.symmetric(
                            horizontal: 15,
                            vertical: 10,
                          ),
                          decoration: BoxDecoration(
                            gradient: msg['isMe']
                                ? AppColors.primaryGradient
                                : null,
                            color: msg['isMe'] ? null : AppColors.cardDark,
                            borderRadius: BorderRadius.circular(15),
                          ),
                          child: Text(
                            msg['text'],
                            style: GoogleFonts.cairo(
                              color: Colors.white,
                              fontSize: 15,
                            ),
                          ),
                        ),
                      );
                    },
                  ),
          ),
          Container(
            padding: const EdgeInsets.all(15),
            decoration: BoxDecoration(
              color: AppColors.cardDark,
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.2),
                  blurRadius: 10,
                ),
              ],
            ),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _controller,
                    style: GoogleFonts.cairo(color: Colors.white),
                    decoration: InputDecoration(
                      hintText: 'اكتب رسالة...',
                      hintStyle: GoogleFonts.cairo(color: AppColors.textGrey),
                      filled: true,
                      fillColor: AppColors.background,
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(25),
                        borderSide: BorderSide.none,
                      ),
                    ),
                    onSubmitted: (_) => _sendMessage(),
                  ),
                ),
                const SizedBox(width: 10),
                GestureDetector(
                  onTap: _sendMessage,
                  child: Container(
                    padding: const EdgeInsets.all(12),
                    decoration: const BoxDecoration(
                      gradient: AppColors.primaryGradient,
                      shape: BoxShape.circle,
                    ),
                    child: const Icon(Icons.send, color: Colors.white),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

// ==================== MAPS SCREEN ====================
class MapsScreen extends StatelessWidget {
  const MapsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      appBar: AppBar(
        backgroundColor: AppColors.cardDark,
        title: Text('الخرائط', style: GoogleFonts.cairo()),
      ),
      body: Stack(
        children: [
          Container(
            color: Colors.grey[300],
            child: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  const Icon(Icons.map, size: 100, color: AppColors.primary),
                  const SizedBox(height: 20),
                  Text(
                    'خريطة Offline',
                    style: GoogleFonts.cairo(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: Colors.black87,
                    ),
                  ),
                ],
              ),
            ),
          ),
          Positioned(
            right: 15,
            bottom: 100,
            child: Column(
              children: [
                FloatingActionButton(
                  heroTag: 'location',
                  onPressed: () {},
                  backgroundColor: AppColors.primary,
                  child: const Icon(Icons.my_location),
                ),
                const SizedBox(height: 10),
                FloatingActionButton(
                  heroTag: 'zoom_in',
                  mini: true,
                  onPressed: () {},
                  backgroundColor: AppColors.cardDark,
                  child: const Icon(Icons.add),
                ),
                const SizedBox(height: 5),
                FloatingActionButton(
                  heroTag: 'zoom_out',
                  mini: true,
                  onPressed: () {},
                  backgroundColor: AppColors.cardDark,
                  child: const Icon(Icons.remove),
                ),
              ],
            ),
          ),
          Positioned(
            top: 15,
            left: 15,
            right: 15,
            child: Container(
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: AppColors.success.withOpacity(0.9),
                borderRadius: BorderRadius.circular(10),
              ),
              child: Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  const Icon(Icons.offline_pin, color: Colors.white, size: 20),
                  const SizedBox(width: 10),
                  Text(
                    'الخرائط متاحة بدون إنترنت',
                    style: GoogleFonts.cairo(
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}

// ==================== SETTINGS SCREEN ====================
class SettingsScreen extends StatelessWidget {
  const SettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      appBar: AppBar(
        backgroundColor: AppColors.cardDark,
        title: Text('الإعدادات', style: GoogleFonts.cairo()),
      ),
      body: ListView(
        padding: const EdgeInsets.all(15),
        children: [
          Container(
            padding: const EdgeInsets.all(20),
            decoration: BoxDecoration(
              color: AppColors.cardDark,
              borderRadius: BorderRadius.circular(15),
            ),
            child: Row(
              children: [
                const CircleAvatar(
                  radius: 30,
                  backgroundColor: AppColors.primary,
                  child: Icon(Icons.person, size: 35),
                ),
                const SizedBox(width: 15),
                Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'مستخدم',
                      style: GoogleFonts.cairo(
                        color: Colors.white,
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    Text(
                      'user@example.com',
                      style: GoogleFonts.cairo(
                        color: AppColors.textGrey,
                        fontSize: 14,
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
          
          const SizedBox(height: 20),
          
          _buildSettingItem(
            icon: Icons.download,
            title: 'التحميل التلقائي',
            subtitle: 'تحميل المحتوى عند الاتصال بـ WiFi',
            trailing: Switch(
              value: true,
              onChanged: (val) {},
              activeColor: AppColors.primary,
            ),
          ),
          
          _buildSettingItem(
            icon: Icons.storage,
            title: 'المساحة المستخدمة',
            subtitle: '2.5 GB من 10 GB',
          ),
          
          _buildSettingItem(
            icon: Icons.info,
            title: 'عن التطبيق',
            subtitle: 'الإصدار 1.0.0',
          ),
          
          const SizedBox(height: 20),
          
          ElevatedButton(
            onPressed: () {
              Navigator.of(context).pushAndRemoveUntil(
                MaterialPageRoute(builder: (_) => const LoginScreen()),
                (route) => false,
              );
            },
            style: ElevatedButton.styleFrom(
              backgroundColor: AppColors.error,
              minimumSize: const Size(double.infinity, 50),
              shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(10),
              ),
            ),
            child: Text(
              'تسجيل الخروج',
              style: GoogleFonts.cairo(
                fontWeight: FontWeight.bold,
                fontSize: 16,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSettingItem({
    required IconData icon,
    required String title,
    required String subtitle,
    Widget? trailing,
  }) {
    return Container(
      margin: const EdgeInsets.only(bottom: 10),
      padding: const EdgeInsets.all(15),
      decoration: BoxDecoration(
        color: AppColors.cardDark,
        borderRadius: BorderRadius.circular(15),
      ),
      child: Row(
        children: [
          Icon(icon, color: AppColors.primary),
          const SizedBox(width: 15),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  title,
                  style: GoogleFonts.cairo(
                    color: Colors.white,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                Text(
                  subtitle,
                  style: GoogleFonts.cairo(
                    color: AppColors.textGrey,
                    fontSize: 12,
                  ),
                ),
              ],
            ),
          ),
          if (trailing != null) trailing,
        ],
      ),
    );
  }
}
