---
layout: default
title: 코딩-2 [해시]
parent: 코딩 문제
---

<br>
출처: <https://programmers.co.kr/learn/courses/30/lessons/42579?language=javascript>

<br>
**문제 설명**
<br>

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.
<br>

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.
   <br>

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

<br>

**문제풀이**

```js
function solution(genres, plays) {
  const data = genres.reduce((obj, genre, i) => {
    if (obj[genre]) {
      obj[genre].total += plays[i];
      obj[genre].numbers.push(i);
    } else {
      obj[genre] = { total: plays[i], numbers: [i] };
    }

    obj[genre].numbers.sort((a, b) => {
      if (plays[b] !== plays[a]) {
        return plays[b] - plays[a];
      } else {
        return a - b;
      }
    });

    return obj;
  }, {});

  const sortData = Object.values(data).sort((a, b) => b.total - a.total);

  return sortData.reduce((arr, b) => {
    if (b.numbers.length !== 1) {
      arr.push(b.numbers[0], b.numbers[1]);
    } else {
      arr.push(b.numbers[0]);
    }

    return arr;
  }, []);
}
```
