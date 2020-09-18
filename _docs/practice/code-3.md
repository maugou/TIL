---
layout: default
title: 코딩-3 [정렬]
parent: 코딩 문제
---

<br>
출처: <https://programmers.co.kr/learn/courses/30/lessons/42747?language=javascript>

<br>
**문제 설명**
<br>

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과에 따르면, H-Index는 다음과 같이 구합니다.
<br>

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.
<br>

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

<br>

**문제풀이**

```js
function solution(citations) {
  let result = 0;
  for (let i = citations.length; i > 0; i--) {
    const arr = citations.filter((n) => i <= n);
    if (arr.length >= i) {
      result = i;
      break;
    }
  }

  return result;
}
```

**다른 사람의 풀이**

```js
function solution(citations) {
  let sorted = citations.slice().sort((a, b) => b - a);
  let i = 0;
  while (i + 1 <= sorted[i]) {
    i++;
  }
  return i;
}
```
