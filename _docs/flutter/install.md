---
layout: default
title: 프로젝트 준비
parent: Flutter
---

## flutter 설치
```
brew install --casks flutter
```
*[공식 문서](https://docs.flutter.dev/get-started/install/macos)에는 brew 안내 x

### 종속성 확인
```
flutter doctor
```



## troubleshooting

**flutter doctor 실행 시 발생**
```
[!] Android toolchain - develop for Android devices (Android SDK version
    33.0.0)
    ✗ cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
    ✗ Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/docs/get-started/install/macos#android-setup for
      more details.
```

  android-studio > settings > Languages & Frameworks > AndroidSdk > SDK Tools<br />
  해당 경로에서 Android SDK Command-line Tools(latest) 설치

  ```
  // command-line tools 설치 후 실행
  flutter doctor --android-licenses
  ```


