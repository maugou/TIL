---
layout: default
title: IOS 앱 storage
parent: 기타
---

## IOS 앱 내 storage 폴더/파일 확인하기

1. Xcode - window - Devices and Simulators
  - INSTALLED APPS 항목에서 앱 선택
  - 설정 아이콘에서 Download Container 선택하여 Local에 저장

2. xcapdata 확장자로 다운로드된 파일 오른쪽 클릭
  - 패키지 내용보기 선택

3. AppData - Documents 내 async-storage 통한 저장된 파일 확인 가능

위 확인이 필요한 경우는 stroage 라이브러리의 저장소가 변경 될 경우(또는 서로 다른 라이브러리 사용할 경우) 저장되는 폴더가 간혹 변경되는 경우 발생하기 때문에 migration 함수 작성 시 참고할 수 있다.
