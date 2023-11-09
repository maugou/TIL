---
layout: default
title: Flutter
has_children: true
has_toc: false
---

- [Class 참고](https://api.flutter.dev/flutter/material/material-library.html#classes)

**위젯** : 화면을 그리는 모든 디자인 요소

### StatelessWidget

state를 가지지 않는 위젯을 구성하는 기본 클래스 ( 한번 그린 후 다시 그리지 않는 경우 )

**method**

- build() : 위젯 생성 시 호출되며, 화면에 그릴 위젯을 작성하여 반환

<br/>

### StatefulWidget

**method**

- createState() : statefulWidget 생성 시 실행

<br/>

### MaterialApp

**기본 형태**<br/>
material 디자인을 사용할 때 기본 구성

![](../../../images/flutter/basic-material-app.png)
