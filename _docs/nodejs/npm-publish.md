---
layout: default
title: NPM 패키지 배포
parent: NodeJS
---

**과정**<br>
- [npm init] package.json 생성 후 초기 설정 진행<br>
  scoped public package 권장<br> 
- [패키지 코드] ts일 경우, tsc로 js변환 (tsconfig.json 설정)
- [로컬 테스트] npm i ./my_module로 설치 후 실행
- [npm login] 배포 전 로그인 필요, 확인 명령어는 npm whoami
- [npm publish --access public] 배포 

#### **참고**
1. <https://docs.npmjs.com/packages-and-modules/> > Contributing packages to the registry
2. <https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html> > bundling with your npm package

<br>

**권장 폴더구조** <br>
권장사항일 뿐, 배포코드 및 본코드를 담는 폴더명은 원하는 것으로 하자.

```
my_module/
 - dist/                  // 배포코드 
 - lib/                   // 본코드
 - .gitignore
 - .npmignore
 - LICENSE
 - package.json
 - README.md
```

**package.json**

옵션 | 설명
-- | --
`name` | 패키지명
`version` | 패키지 버전 (배포 시 동일한 버전으로 중복 배포 되지 않는다.)
`description` | [npmjs](https://www.npmjs.com/) 에서 검색 시 패키지명 아래 설명줄
`keyword` | 패키지 상세페이지에 표기되는 검색어
`bugs` | 패키지의 이슈 등을 볼수 있는 url, 이슈를 알릴 email 주소 입력(하나만 적용 가능)
`license` | 패키지의 라이센스
`repository` | 코드 저장소 위치
