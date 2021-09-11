---
layout: default
title: 코딩-4
parent: 코딩 문제
---

<br>
출처: <https://leetcode.com/problems/add-two-numbers/>

<br>
**문제 설명**
<br>

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself

<br>

**문제풀이**

```js
const solution = (l1, l2) => {
  let numbers = [];
  for (const arr of [l1, l2]) {
    arr.reverse();
    numbers.push(Number(arr.join("")));
  }

  const sumNumber = numbers.reduce((acc, cur) => acc + cur);

  const result = sumNumber
    .toString()
    .split("")
    .map((c) => Number(c));

  result.reverse();

  return result;
};
```
