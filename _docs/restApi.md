---
layout: default
title: REST API
---

# **REST API**

---

### URI는 자원을 표현한다.<br>
- 동사보다는 명사를 사용한다. 즉, 행위보다 자원을 표현
 ```
 /users   // 권장
 /delete // 권장 x
 ```
<br>

### 자원에 대한 행위는 HTTP 메소드로 표현한다.
- GET(조회), POST(생성), PUT(수정), DELETE(삭제)로 표현한다.

<br>

### 추가적인 권장 사항
밑줄(_)은 사용하지 않는다.<br>
URI 경로는 소문자로 작성한다.<br>
URI 마지막에 (/)를 사용하지 않는다.