## Struktur Proyek

```
lib/
├── provider/
│   └── audio_notifier.dart
├── screen/
│   └── home_screen.dart
├── utils/
│   └── utils.dart
├── widget/
│   ├── audio_controller_widget.dart
│   └── buffer_slider_controller_widget.dart
└── main.dart
```

## audio_notifier.dart

```dart
class AudioNotifier extends ChangeNotifier { // import flutter.material.dart
  bool _isPlay = false;
  Duration _duration = Duration.zero;
  Duration _position = Duration.zero;

  bool get isPlay => _isPlay;
  set isPlay(bool value) {
    _isPlay = value;
    notifyListeners();
  }

  Duration get duration => _duration;
  set duration(Duration value) {
    _duration = value;
    notifyListeners();
  }

  Duration get position => _position;
  set position(Duration value) {
    _position = value;
    notifyListeners();
  }
}
```

## audio_controller_widget.dart

```dart
class AudioControllerWidget extends StatelessWidget { // import flutter.material.dart
  final Function onPlayTapped;
  final Function onStopTapped;
  final Function onPauseTapped;
  final bool isPlay;

  const AudioControllerWidget({
    super.key,
    required this.onPlayTapped,
    required this.onStopTapped,
    required this.onPauseTapped,
    required this.isPlay,
  });

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(10),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceAround,
        children: [
          IconButton(
            onPressed: () {
              if (isPlay) {
                onPauseTapped();
              } else {
                onPlayTapped();
              }
            },
            icon: Icon(
              isPlay ? Icons.pause : Icons.play_arrow,
            ),
            iconSize: 30,
          ),
          IconButton(
            onPressed: () => onStopTapped(),
            icon: const Icon(Icons.stop),
            iconSize: 30,
          ),
        ],
      ),
    );
  }
}
```

## buffer_slider_controller_widget.dart 

```dart
class BufferSliderControllerWidget extends StatelessWidget { // import flutter.material.dart
  final String minText;
  final String maxText;
  final double maxValue;
  final double currentValue;
  final Function(double) onChanged;

  const BufferSliderControllerWidget({
    super.key,
    required this.minText,
    required this.maxText,
    required this.maxValue,
    required this.currentValue,
    required this.onChanged,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Slider(
          value: currentValue,
          min: 0,
          max: maxValue,
          onChanged: (value) => onChanged(value),
        ),
        Padding(
          padding: const EdgeInsets.symmetric(horizontal: 16),
          child: Row(
            mainAxisSize: MainAxisSize.max,
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(minText),
              Text(maxText),
            ],
          ),
        ),
      ],
    );
  }
}
```

## home_screen.dart

```dart
class HomeScreen extends StatefulWidget {  // import flutter.material.dart
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Audio Player Project")),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
         // import widget/buffer_slider_controller_widget.dart
          BufferSliderControllerWidget( 
            maxValue: 5,
            currentValue: 1,
            minText: "Current Duration",
            maxText: "Maximum Duration",
            onChanged: (value) async {},
          ),
          // import widget/audio_controller_widget.dart
          AudioControllerWidget(
            onPlayTapped: () {},
            onPauseTapped: () {},
            onStopTapped: () {},
            isPlay: false,
          ),
        ],
      ),
    );
  }
}
```

## utils.dart 

```dart
durationToTimeString(Duration value) {
  final minute = value.inMinutes % 60;
  final second = value.inSeconds % 60;

  final minuteString = minute.toString().padLeft(2, '0');
  final secondString = second.toString().padLeft(2, '0');

  return "$minuteString:$secondString";
}
```

## main.dart 

```dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget { // import material.dart
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    // import provider.dart
    // provider/audio_notifier.dart
    // screen/home_screen.dart
    return ChangeNotifierProvider(
      create: (context) => AudioNotifier(),
      child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(primarySwatch: Colors.blue),
        home: const HomeScreen(),
      ),
    );
  }
}
```