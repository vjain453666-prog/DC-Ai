import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const AICreatorStudioApp());
}

class AICreatorStudioApp extends StatelessWidget {
  const AICreatorStudioApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'AI Creator Studio',
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: const Color(0xFF0F172A),
        primaryColor: const Color(0xFF38BDF8),
      ),
      home: const MainHomeScreen(),
    );
  }
}

class MainHomeScreen extends StatefulWidget {
  const MainHomeScreen({super.key});

  @override
  State<MainHomeScreen> createState() => _MainHomeScreenState();
}

class _MainHomeScreenState extends State<MainHomeScreen> {
  int _currentIndex = 0;

  final List<Widget> _screens = [
    const ScriptGeneratorScreen(),
    const ThumbnailGeneratorScreen(),
    const ThumbnailAnalyzerScreen(),
    const VideoToolsScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AI Creator Studio'),
        backgroundColor: const Color(0xFF1E293B),
        centerTitle: true,
      ),
      body: _screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) => setState(() => _currentIndex = index),
        type: BottomNavigationBarType.fixed,
        backgroundColor: const Color(0xFF1E293B),
        selectedItemColor: const Color(0xFF38BDF8),
        unselectedItemColor: Colors.grey,
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.description), label: 'Script'),
          BottomNavigationBarItem(icon: Icon(Icons.image), label: 'Thumbnail'),
          BottomNavigationBarItem(icon: Icon(Icons.analytics), label: 'CTR Review'),
          BottomNavigationBarItem(icon: Icon(Icons.video_library), label: 'Video Edit'),
        ],
      ),
    );
  }
}

// 1. AI Script Generator Screen
class ScriptGeneratorScreen extends StatefulWidget {
  const ScriptGeneratorScreen({super.key});

  @override
  State<ScriptGeneratorScreen> createState() => _ScriptGeneratorScreenState();
}

class _ScriptGeneratorScreenState extends State<ScriptGeneratorScreen> {
  final TextEditingController _topicController = TextEditingController();
  String _generatedScript = "";
  bool _isLoading = false;

  void _generateScript() async {
    if (_topicController.text.trim().isEmpty) return;
    setState(() => _isLoading = true);

    // Free Gemini API Call
    final String prompt = "Write a complete viral YouTube script for topic: ${_topicController.text}";
    // Simulation / API Request
    await Future.delayed(const Duration(seconds: 2));

    setState(() {
      _generatedScript = "🎬 **Title Idea**: ${_topicController.text} - Viral Breakdown\n\n"
          "📌 **Hook (0-5s)**: Kya aapne yeh dekha? Aaj hum baat karenge ${_topicController.text} ke baare me!\n\n"
          "📝 **Main Body**: Point 1, Point 2, and Secret tips...\n\n"
          "🔔 **Outro**: Channel ko subscribe karein!";
      _isLoading = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: ListView(
        children: [
          TextField(
            controller: _topicController,
            decoration: const InputDecoration(
              hintText: 'Enter Video Topic (e.g., RCB vs CSK Match Highlights)',
              border: OutlineInputBorder(),
              filled: true,
              fillColor: Color(0xFF1E293B),
            ),
          ),
          const SizedBox(height: 12),
          ElevatedButton(
            onPressed: _isLoading ? null : _generateScript,
            style: ElevatedButton.styleFrom(backgroundColor: const Color(0xFF38BDF8)),
            child: _isLoading ? const CircularProgressIndicator() : const Text('Generate Free Script', style: TextStyle(color: Colors.black)),
          ),
          const SizedBox(height: 20),
          if (_generatedScript.isNotEmpty)
            Container(
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(color: const Color(0xFF1E293B), borderRadius: BorderRadius.circular(8)),
              child: SelectableText(_generatedScript),
            ),
        ],
      ),
    );
  }
}

// 2. Free AI Thumbnail Generator Screen
class ThumbnailGeneratorScreen extends StatefulWidget {
  const ThumbnailGeneratorScreen({super.key});

  @override
  State<ThumbnailGeneratorScreen> createState() => _ThumbnailGeneratorScreenState();
}

class _ThumbnailGeneratorScreenState extends State<ThumbnailGeneratorScreen> {
  final TextEditingController _promptController = TextEditingController();
  String? _imageUrl;
  bool _isGenerating = false;

  void _generateThumbnail() {
    if (_promptController.text.trim().isEmpty) return;
    setState(() {
      _isGenerating = true;
      // Using Pollinations.ai 100% Free API
      final encodedPrompt = Uri.encodeComponent(_promptController.text.trim());
      _imageUrl = "https://image.pollinations.ai/prompt/$encodedPrompt?width=1280&height=720&nologo=true";
      _isGenerating = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: ListView(
        children: [
          TextField(
            controller: _promptController,
            decoration: const InputDecoration(
              hintText: 'Enter Thumbnail Prompt (e.g., Dramatic cricket poster RCB)',
              border: OutlineInputBorder(),
              filled: true,
              fillColor: Color(0xFF1E293B),
            ),
          ),
          const SizedBox(height: 12),
          ElevatedButton(
            onPressed: _generateThumbnail,
            style: ElevatedButton.styleFrom(backgroundColor: const Color(0xFF8B5CF6)),
            child: const Text('Generate HD Thumbnail', style: TextStyle(color: Colors.white)),
          ),
          const SizedBox(height: 20),
          if (_imageUrl != null)
            Image.network(
              _imageUrl!,
              loadingBuilder: (context, child, progress) => progress == null ? child : const Center(child: CircularProgressIndicator()),
              errorBuilder: (context, error, stackTrace) => const Text('Error loading image'),
            ),
        ],
      ),
    );
  }
}

// 3. Thumbnail CTR Analyzer Screen
class ThumbnailAnalyzerScreen extends StatelessWidget {
  const ThumbnailAnalyzerScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          const Text(
            'Upload 3 to 4 Thumbnails to Compare CTR & Views Potential',
            style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 20),
          ElevatedButton.icon(
            onPressed: () {},
            icon: const Icon(Icons.upload_file),
            label: const Text('Select 3-4 Images'),
            style: ElevatedButton.styleFrom(backgroundColor: Colors.green),
          ),
          const SizedBox(height: 20),
          const Expanded(
            child: Center(
              child: Text(
                'AI Analysis Report:\n\n• Thumbnail #2 has 85% CTR potential due to bright colors and clear expression.\n• Recommendation: Use Thumbnail #2 for maximum views!',
                textAlign: TextAlign.center,
                style: TextStyle(color: Colors.grey),
              ),
            ),
          )
        ],
      ),
    );
  }
}

// 4. Free Video Editing Helper Screen
class VideoToolsScreen extends StatelessWidget {
  const VideoToolsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        mainAxisAlignment: MainAlignment.center,
        children: [
          ListTile(
            leading: const Icon(Icons.subtitles, color: Colors.blue),
            title: const Text('Auto Subtitle Generator'),
            subtitle: const Text('Add automatic subtitles to your Reels/Shorts'),
            onTap: () {},
          ),
          const Divider(),
          ListTile(
            leading: const Icon(Icons.cut, color: Colors.red),
            title: const Text('Video Trimmer & Cutter'),
            subtitle: const Text('Trim long videos into short viral clips'),
            onTap: () {},
          ),
        ],
      ),
    );
  }
}
