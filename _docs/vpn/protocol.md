---
layout: default
title: Protocol
parent: VPN
---

# Protocol

---

### 프로토콜이란 ?

컴퓨터나 통신 장비가 서로 소통하기 위해 정한 규약<br />

- 사람 간 의사소통에서도 언어가 일치해야 소통 가능 -> 네트워크 업계에서는 공통 프로토콜 선정을 위해 IEEE나 IETF 같은 표준화 단체 존재

### TCP와 UDP의 차이

|      | TCP (전송 제어 프로토콜)                             | UDP (사용자 데이터그램 프로토콜)                     |
| ---- | ---------------------------------------------------- | ---------------------------------------------------- |
| 방식 | 데이터 형태 그대로 전달 및 송수신 확인 (신뢰도 높음) | 단순히 전달 (신뢰도 낮음)                            |
| 속도 | UDP 대비 느림                                        | TCP 대비 빠름                                        |
| 기타 | 순서 및 안정성이 우선시 될 경우 유리                 | 손실, 중복 등이 허용되며, 속도를 우선시 할 경우 유리 |

### Wireguard 프로토콜 속도가 빠른 이유

- 코드 양이 다른 프로토콜에 비해 적다. (약 4000줄)
- UDP 사용

### kernel space 와 user space 차이

| kernel space                                  | user space                                    |
| --------------------------------------------- | --------------------------------------------- |
| 운영체제를 실행시키기 위해 필요한 메모리 공간 | 프로그램이 동작하기 위해 사용되는 메모리 공간 |

위와 같이 나눈 이유는 일반 사용자가 운영체제를 실행시키기 위한 자원에 접근하지 못하도록 구분