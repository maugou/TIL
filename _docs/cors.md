---
layout: default
title: CORS
---

# **CORS**

---

![](../../images/no-cors.png)

서로 다른 도메인 또는 포트로 리소스를 요청할 때 CORS를 설정하지 않으면 에러가 발생한다.

Ajax 통신 시 same-origin 정책에 의해 동일한 도메인과 포트로만 요청, 응답할 수 있기 때문이다.<br>
즉, CORS는 다른 도메인 또는 포트로도 리소스를 요청할 수 있도록 해준다.

<br>

## 서버 측에서 CORS를 허용하는 방법

### Access-Control-Allow-Origin 응답 헤더

서버 응답에서 접근 권한을 주는 것.

- Access-Control-Allow-Origin: \* // 모든 도메인
- Access-Control-Allow-Origin: 'http://example.com' // 특정 도메인

헤더에 추가해서 응답하면 CORS를 허용한다.

<br>

### Express 기반 서버 CORS 허용

Express 미들웨어 모듈을 설치 및 사용한다.

```
npm install cors
```

<br>
설치 후 아래와 같이 간단하게 사용할 수 있다.

```
var express = require('express')
var cors = require('cors')
var app = express()

app.use(cors())
```

특정 경로의 요청에만 CORS를 허용하는 등의 다른 설정은 [여기](https://github.com/expressjs/cors)를 참고하자.

<br>

### hapi 기반 서버 CORS 허용

내부 옵션을 이용한다.

```
const server = Hapi.server({
  port: 5000,
  host: "0.0.0.0",
  routes: {
    cors: true
  }
});
```

또는, 아래와 같이 특정 경로의 요청에만 CORS 허용도 가능하다.

```
server.route({
    method: 'GET',
    path: '/',
    options: {
        cors: true,
        handler: (req, h) => {
              ...
        }
    }
});
```

추가적인 설정은 [공식 문서](https://hapi.dev/api/?v=19.1.1#-routeoptionscors)를 참조하자
