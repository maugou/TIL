---
layout: default
title: Typescript
---

# **타입스크립트**

---

**tsconfig.json**

`compilerOptions` <br>
출처: <https://www.typescriptlang.org/docs/handbook/compiler-options.html>

| 옵션                             | 타입    | 기본값                                                     | 설명                                                                                                          |
| -------------------------------- | ------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `--outDir`                       | string  |                                                            | 해당폴더로 전송(출력)                                                                                         |
| `--jsx`                          | string  | "preserve"                                                 | .tsx 파일에서 JSX 지원: "react", "preserve", "react-native" 중 선택                                           |
| `--target`                       | string  | "ES3"                                                      | "None", "CommonJS", "AMD", "System", "UMD", "ES6", "ES2015" or "ESNext" 중 선택                               |
| `--esModuleInterop`              | boolean | false                                                      | `allowSyntheticDefaultImports`를 활성화                                                                       |
| `--allowSyntheticDefaultImports` | boolean | module === "system" or `--esModuleInterop`                 | `default export`가 없는 모듈에서 `default imports`를 허용. 코드 방출에는 영향을 주지 않으며, 타입 검사만 수행 |
| `--module`                       | string  | target === "ES3" or "ES5" ? "CommonJS" : "ES6"             | "None", "CommonJS", "AMD", "System", "UMD", "ES6", "ES2015" 또는 "ESNext" 중 선택                             |
| `--moduleResolution`             | string  | module === "AMD" or "System" or "ES6" ? "Classic" : "Node" | 모듈 해석 방법 결정                                                                                           |

`include`: [ ]<br>
포함된 것만 컴파일 (빈 값일 경우 exclude 제외 모든 파일)<br>
`exclude`: [ ]<br>
컴파일 제외 (빈 값일 경우 `--outDir` 제외)
