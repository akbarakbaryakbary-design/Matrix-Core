import 'package:flutter/material.dart';
import 'dart:ui';

void main() {
  runApp(const UltraMatrixScannerApp());
}

class UltraMatrixScannerApp extends StatelessWidget {
  const UltraMatrixScannerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Matrix Ultra Scanner',
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: const Color(0xFF030305), // مشکی مطلق لوکس
        primaryColor: const Color(0xFF00FFCC), // سبز نئونی فوق‌سریع
      ),
      home: const PremiumScannerScreen(),
    );
  }
}

class PremiumScannerScreen extends StatefulWidget {
  const PremiumScannerScreen({super.key});

  @override
  State<PremiumScannerScreen> createState() => _PremiumScannerScreenState();
}

class _PremiumScannerScreenState extends State<PremiumScannerScreen> with SingleTickerProviderStateMixin {
  late AnimationController _scannerController;

  @override
  void編init() {
    super.initState();
    // انیمیشن خط اسکنر با فریم‌ریت بالا برای سرعت جنون‌آمیز
    _scannerController = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    )..repeat(reverse: true);
  }

  @override
  void dispose() {
    _scannerController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Stack(
        children: [
          // ۱. شبیه‌ساز نمای دوربین فوق‌سریع در پس‌زمینه
          Container(
            decoration: const BoxDecoration(
              gradient: LinearGradient(
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
                colors: [Color(0xFF0F0C20), Color(0xFF060610)],
              ),
            ),
            child: const Center(
              child: Icon(Icons.photo_camera_front, size: 80, color: Color(0x2200FFCC)),
            ),
          ),

          // ۲. لایه شیشه‌ای لوکس و مینیمال (Glassmorphism)
          Positioned.fill(
            child: BackdropFilter(
              filter: ImageFilter.blur(sigmaX: 10, sigmaY: 10),
              child: Container(color: Colors.black.withOpacity(0.3)),
            ),
          ),

          // ۳. کادر هوشمند و خط اسکن متحرک نئونی
          Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Container(
                  width: 280,
                  height: 280,
                  decoration: BoxDecoration(
                    border: Border.all(color: const Color(0xFF00FFCC), width: 2),
                    borderRadius: BorderRadius.circular(24),
                    boxShadow: [
                      BoxShadow(
                        color: const Color(0xFF00FFCC).withOpacity(0.15),
                        blurRadius: 20,
                        spreadRadius: 2,
                      )
                    ],
                  ),
                  child: AnimatedBuilder(
                    animation: _scannerController,
                    builder: (context, child) {
                      return Stack(
                        children: [
                          Positioned(
                            top: _scannerController.value * 270,
                            left: 10,
                            right: 10,
                            child: Container(
                              height: 4,
                              decoration: const BoxDecoration(
                                color: Color(0xFF00FFCC),
                                boxShadow: [
                                  BoxShadow(
                                    color: Color(0xFF00FFCC),
                                    blurRadius: 12,
                                    spreadRadius: 3,
                                  )
                                ],
                              ),
                            ),
                          ),
                        ],
                      );
                    },
                  ),
                ),
                const SizedBox(height: 35),
                // متن وضعیت
                const Text(
                  'MATRIX SCANNER ENGINE ACTIVE',
                  style: TextStyle(
                    color: Color(0xFF00FFCC),
                    fontSize: 14,
                    fontWeight: FontWeight.bold,
                    letterSpacing: 3,
                  ),
                ),
                const SizedBox(height: 8),
                Text(
                  'آماده اسکن بر روی تمامی پلتفرم‌ها',
                  style: TextStyle(
                    color: Colors.white.withOpacity(0.6),
                    fontSize: 13,
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
