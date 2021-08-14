---
layout: default
title: Android
---

# **Android**

### App 모듈 구성

- manifest, java, res 등의 폴더 생성
- 그 외 test관련 파일 및 빌드 구성 파일 등이 생성

### manifest

- AndroidManifest.xml 파일 (필수 파일)
- AndroidManifest 파일은 앱의 패키지명 및 권한에 대한 설정, 앱의 구성에 대한 내용을 담고 있어야 한다.
- build.gradle(project)의 applicationID와 AndroidManifest의 패키지명이랑 대응하기 때문에 양쪽의 이름은 같은 이름을 권장하며, 이것은 출시 후 고유 앱으로 식별한다.

### res

- 코드에서 추가되는 파일들의 모음
  - drawable : 화면에 그려지는 속성 (.png, .jpg , .gif 또는 XML 파일)
  - layout: 사용자 인터페이스 레이아웃 (XML 파일)
  - mipmap: 아이콘 밀도에 대한 drawable 파일
  - values: 텍스트 스타일 지정 및 서식 지정

### Gradle

- 전체 app에 대한 gradle 파일과 각 모듈 별 gradle 파일이 생성되어진다.
- seettings.gradle파일의 설정에 따라 실행될 모듈이 정해진다. 해당 모듈의 gradle 파일을 읽는다.

---

- 모듈

  기본 모듈 및 라이브러리 모듈을 생성하거나 가져 올 수 있다.
  기본 모듈과 라이브러리 모듈의 차이는 빌드 후 파일의 형태 및 사용하고자 하는 의도에서 차이점을 가진다.
  전체 빌드 후 APK가 생성이 되고 기본 모듈은 JAR을 라이브러리 모듈은 AAR형태의 파일 생성한다. APK안에 두 파일이 존재하되, AAR은 단독으로 다른 App파일에 가져다 쓸 수 있다.

- JAR이란 ? AAR 이란?

  JAR은 소스코드를 컴파일 이후 java바이트 코드를 zip archive 형태로 가지는 것
  AAR 은 JAR과 다르게 리소스 파일도 가지고 있다.

- APK란?

  android 플랫폼에 배포할 수 있는 파일 형태이며 빌드 되어진 상태를 칭한다. 빌드 과정은 컴파일진행까지 모두 포함된다.

- 새로운 모듈을 만드는 과정에서 설정 양식에 대한 정보
  - application Name: 아이콘 제목으로 사용된다.
    - AndroidManifest 파일에서 application요소에 label 속성에 내용으로 사용된다.
  - Module Name: 모듈을 나타내는 폴더 이름.
  - Package Name: Android manifest 파일의 패키지명
  - Minimum SDK: 지원하는 android OS 버전(?)의 가장 낮은 버전. build.gradle 파일의 minSdkVersion 속성이다. 편집 가능.

---

### Activity

manifest에 activity 요소 선언해야만 사용 가능

**생명 주기**<br/>
![](../../../images/android/activity_lifecycle.png)
