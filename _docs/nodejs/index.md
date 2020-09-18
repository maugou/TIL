---
layout: default
title: NodeJS
has_children: true
has_toc: false
---

# **NodeJS**

---

자바스크립트 언어를 웹 이외에 사용할 수 있도록 해주는 JS 런타임이다.

## n

NodeJS 버전관리 모듈이다.

- 프로젝트별 NodeJS 버전이 다를 경우 유용하다.

설치

```
brew install n
```

환경변수 설정

```
export N_PREFIX=$HOME/.n
export PATH=$N_PREFIX/bin:$PATH
```

<br>

## npm(패키지 관리도구) 명령어

npm init : package.json 생성

- 의존성 모듈 관리 환경 형성한다. <br>

npm install : 모듈 설치

- npm ver5 이후부터 --save 생략 가능하다.
- devDependencies 추가는 -D 또는 --save-dev<br>

npm update : 모듈 버전 업데이트<br>
npm outdated : 업데이트 가능한 모듈 표시<br>
npm cache clean

### 모듈의 변경이 있을 때 오류가 생기거나 이전 상황이 그대로 연출 될때 대처 방법

1. Clear watchman watches: watchman watch-del-all
2. Delete node_modules: rm -rf node_modules and run npm install
3. Reset Metro's cache: npm start --reset-cache
4. Remove the cache: rm -rf /tmp/metro-\*

## npx

npm 5.2.0 버전부터 추가된 도구

- npx로 모듈 실행 시 우선적으로 해당 폴더에 실행파일의 유무를 파악<br>
  만약, 지역설치로 해당 프로젝트에 설치된 모듈이라면 실행파일 실행 <br>
  미설치된 모듈이라면 해당 저장소를 통해 설치 저장없이 실행
