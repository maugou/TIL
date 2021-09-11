---
layout: default
title: 코딩-1
parent: 코딩 문제
---

<br>
출처: <https://programmers.co.kr/learn/courses/30/lessons/42578?language=javascript>

<br>
**문제 설명**
<br>

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.
<br>
예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

종류 | 이름
얼굴 | 동그란 안경, 검정 선글라스
상의 | 파란색 티셔츠
하의 | 청바지
겉옷 | 긴 코트

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

<br>

**문제 풀이**

```js
function solution(clothes) {
  const kindOfClothes = clothes
    .map((c) => c[1])
    .reduce((acc, cur) => {
      if (cur in acc) {
        acc[cur]++;
      } else {
        acc[cur] = 1;
      }
      return acc;
    }, {});

  return (
    Object.values(kindOfClothes)
      .map((value) => value + 1)
      .reduce((acc, cur) => acc * cur) - 1
  );
}
```

<br>

**다른 사람의 풀이**

```js
function solution(clothes) {
  return (
    Object.values(
      clothes.reduce((obj, t) => {
        obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 1;
        return obj;
      }, {})
    ).reduce((a, b) => a * (b + 1), 1) - 1
  );
}
```
