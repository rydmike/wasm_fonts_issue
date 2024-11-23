# wasm_fonts_issue

A repo to demonstrate a font loading issue with Flutter WASM and 
Google Fonts.

In WASM builds the Google Fonts are not loaded when using them as implicit assets.



## Expect

Expect Google Fonts to load and be used in a Flutter WEB WASM build using same code and configuration as for native Flutter VM builds and Web JS builds, when using Google Fonts in the Flutter app, bundled as assets.


| WEB JS BUILD | VM MacOS Build  |
|--------------|-----------------|
|              |                 |


The example fonts are loaded from assets correctly.

## Actual WEB WASM Build

The example fonts are not loaded:



## Work around

If you list the individual fonts in the `pubspec.yaml` file, "the classic way" and use them as assets that way, the fonts will be loaded and used in the Flutter WASM build too.

Uncomment the fonts in the project `pubspec.yaml`:

```yaml
#  fonts:
#    - family: Rancho
#      fonts:
#        - asset: assets/google_fonts/Rancho-Regular.ttf
#    - family: FiraMono
#      fonts:
#        - asset: assets/google_fonts/FiraMono-Regular.ttf
```

So it becomes:

```yaml
  fonts:
    - family: Rancho
      fonts:
        - asset: assets/google_fonts/Rancho-Regular.ttf
    - family: FiraMono
      fonts:
        - asset: assets/google_fonts/FiraMono-Regular.ttf
```

Build WEB --wasm again, hot-restart was enough on in my case, fonts are **now** loaded and used correctly.

But the way mentioned in Google Fonts docs here:

https://pub.dev/packages/google_fonts#bundling-fonts-when-releasing

to only put them in assets and have them loaded, does not work.
This does as shown work in both VM and Web JS builds, it only fails in the WASM build.


## Source code

See repo:

The app uses the Google Fonts feature to force loading used fonts as assetts:

```dart
// Only use Google fonts via asset provided fonts.
GoogleFonts.config.allowRuntimeFetching = false;
```

## Flutter version

```consle
[✓] Flutter (Channel master, 3.27.0-1.0.pre.621, on macOS 15.1.1 24B91 darwin-arm64, locale en-US)
    • Flutter version 3.27.0-1.0.pre.621 on channel master at /Users/rydmike/fvm/versions/master
    • Upstream repository https://github.com/flutter/flutter.git
    • Framework revision da188452a6 (55 minutes ago), 2024-11-23 19:55:24 +0100
    • Engine revision b382d17a27
    • Dart version 3.7.0 (build 3.7.0-183.0.dev)
    • DevTools version 2.41.0-dev.2

[✓] Android toolchain - develop for Android devices (Android SDK version 34.0.0)
    • Android SDK at /Users/rydmike/Library/Android/sdk
    • Platform android-34, build-tools 34.0.0
    • Java binary at: /Applications/Android Studio.app/Contents/jbr/Contents/Home/bin/java
      This is the JDK bundled with the latest Android Studio installation on this machine.
      To manually set the JDK path, use: `flutter config --jdk-dir="path/to/jdk"`.
    • Java version OpenJDK Runtime Environment (build 17.0.9+0-17.0.9b1087.7-11185874)
    • All Android licenses accepted.

[!] Xcode - develop for iOS and macOS (Xcode 16.1)
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Build 16B40

[✓] Chrome - develop for the web
    • Chrome at /Applications/Google Chrome.app/Contents/MacOS/Google Chrome

[✓] Android Studio (version 2023.2)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin can be installed from:
    • Dart plugin can be installed from:
    • Java version OpenJDK Runtime Environment (build 17.0.9+0-17.0.9b1087.7-11185874)

[✓] IntelliJ IDEA Community Edition (version 2024.2.4)
    • IntelliJ at /Applications/IntelliJ IDEA CE.app
    • Flutter plugin version 82.1.3
    • Dart plugin version 242.22855.32

[✓] VS Code (version 1.95.3)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.100.0

[✓] Connected device (4 available)
    • MrPinkPro (mobile)              • 74120d6ef6769c3a2e53d61051da0147d0279996 • ios            • iOS 17.7.2 21H221
    • macOS (desktop)                 • macos                                    • darwin-arm64   • macOS 15.1.1 24B91 darwin-arm64
    • Mac Designed for iPad (desktop) • mac-designed-for-ipad                    • darwin         • macOS 15.1.1 24B91 darwin-arm64
    • Chrome (web)                    • chrome                                   • web-javascript • Google Chrome 131.0.6778.86

[✓] Network resources
    • All expected network resources are available.



```

