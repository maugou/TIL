---
layout: default
title: CocoaPods
parent: 기타
---

**에러 현상**

pod install 진행 중 에러 발생
```shell
$ pod install

...

# 에러
xcrun: error: SDK “iphoneos” cannot be located

...
```

**해결**

1. XCode 열고 메뉴바의 XCode - Preferences 선택
2. Locations 탭 선택하여 Command Line Tools 항목에 최신버전으로 선택

이후 pod install 은 정상동작한다.