---
layout: default
title: Debugging
---

# **Debugging**

## Chrome 개발자도구에서 JS 디버깅

개발자도구의 Sources 탭에서 디버깅을 한다.<br>
중단점이 사용되면 Scope창을 통해 할당되어져 있는 값들을 알 수 있다.( console.log보다 시간비용 절감 )

### breakpoint 유형

**코드 줄**

![](../../images/debugging/line-of-code.png)

위 사진처럼 중단하고자 하는 코드 줄을 선택하면 빨간점이 생긴다.<br>
오른쪽 breakpoints에 중단점을 모아볼 수 있다.

**조건부 코드 줄**

코드 줄 중단점 선택 위치에서 오른쪽 클릭 후 Add conditional breakpoint를 선택한다.<br>
해당 코드 줄 아래에 대화상자가 나타나면 조건을 작성하고 완료 시 중단점이 형성된다. 작성한 조건이 참일 경우 중단점이 사용된다.

**이벤트 리스너**

이벤트가 발생하고 리스너 코드에서 중단하고자 할 때 사용한다. ex) 클릭 후 실행되는 코드에서 중단<br>
Event Listener Breakpoints 창에서 이벤트 카테고리 중 특정 이벤트를 선택하여 사용한다.

### 설정

**Blackboxing**

디버깅 시 특정 라이브러리에 대해 디버깅 과정에서 제외시키는 설정 <br>
참조: <https://developers.google.com/web/tools/chrome-devtools/javascript/step-code?hl=ko>

<br>

## VS Code에서 디버깅

node.js 런타임에 대한 디버깅 지원기능이 내장되어 있다.<br>
그래서 JS, TS 또는 JS로 변환되는 다른 언어를 디버깅 가능하다.<br>
Ruby, Python 등등 다른 언어를 디버깅하려면 마켓플레이스를 이용한다.

기본적인 동작흐름은 크롬의 개발자도구에서 디버깅 진행하는 흐름과 유사하다.<br>
다만, 아래 launch.json 파일을 통해 디버깅 관련 정보를 구성하고 진행한다는 점 유의

### launch.json

필수 요소

- type : 사용할 디버거 유형 (내장 node 디버거 등)
- request : 요청 유형 (launch 와 attach 지원)
- name : 디버그 드롭다운에 표시되는 이름
