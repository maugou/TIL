---
layout: default
title: VPN
has_children: true
has_toc: false
---

# VPN

---

### VPN 서비스를 사용하는 이유

- 지역적으로 제한된 콘텐츠에 접근하기 위해 사용
- 개인적 데이터를 보호(암호화) ( ISP가 정보를 판매하는데 사용하기도 한다 )
  - (예시) 어느 제품을 선호하는지 등을 통해 광고에 보여주기도 함

### VPN의 기능

- VPN 은 데이터를 암호화하고 IP 주소를 마스킹하여 사용자의 검색 기록과 위치를 추적할 수 없도록 하여 사용자를 보호

### VPN 프로토콜 종류 및 특징

| 프로토콜 이름 | 특징                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 오픈VPN       | - 오픈 소스 프로토콜인만큼 신뢰성과 보안이 우수<br/> - 속도가 다소 느리다는 평 존재                                                                                            |
| SSTP          | - 윈도우 운영체제 지원 ( Microsoft 개발 )<br/> - 보안 및 방화벽 우회 우수                                                                                                      |
| IKEv2 / IPSec | - 가장 안전한 VPN 프로토콜에 속함<br/> - 속도가 다소 느리다는 평 존재                                                                                                          |
| L2TP/IPSec    | - 윈도우 운영체제 지원 ( Microsoft 개발 )<br/> - 윈도우 외에 운영체제 지원이 아쉬움<br /> - 인터넷 연결 전환에도 안정적 유지가 장점<br />&nbsp; \*모바일 → LTE 와 wifi 변경 등 |
| PPTP          | - 보안이 취약하여 더 이상 권장되지 않는 프로토콜<br/> - 방화벽에 쉽게 차단됨                                                                                                   |
| WireGuard     | - 비교적 최신 프로토콜 → 가장 빠른 프로토콜<br/> - OpenVPN과 동일한 오픈 소스 프로토콜<br/> - 발견되지 않은 취약한 부분이 있을 가능성 → 가장 최신이기 때문                     |

### 모바일 VPN 서비스 요약

|서비스명|특징|요금제|평가|
|CyberGhost|- 노로그 정책<br/> - 장치 7대까지 동시 보호<br/> - 무제한 대역폭 제공으로 속도 우수| - 1개월 : 15300원 / 개월<br />- 6개월 : 9700원 / 개월<br/>- 2년 : 3050원 / 개월|- 가장 많이 사용<br/>- 속도 등으로 인해 게이밍에 최적화 안내<br/>- 장기 요금제 기준으로 저렴|
|NordVPN|- 노로그 정책<br/>- 장치 6대까지 보호<br/>- 가장 빠른 속도 제공|기능별 등급으로 제공<br/>[ 스탠다드 기준 ]<br/>- 1개월 : 16900원 / 개월<br/>- 1년 : 6900원 / 개월<br/>- 2년 : 4500월 / 개월|- 요금제 부분에서 큰 메리트는 없음<br/>- 기본적인 VPN보다 보안과 속도 우수|
|Surfshark|- 노로그 정책<br/>- 장치 무제한 보호|- 30일 환불 보장<br/>- 기능별 등급으로 제공<br/>[ starter 기준 ]<br/>- 1개월 : 20350원 / 개월<br/>- 1년 : 5255원 / 개월<br/>- 2년 : 3027원 / 개월|- 보안 암호화 방식 소개가 없는 관계로 보안 수준 파악 어려움<br/>- 장치가 무제한이지만 다른 VPN에 비해 지원하는 운영체제가 적음<br/>- 가격적인 면에서 장기 사용할 경우 매우 저렴|