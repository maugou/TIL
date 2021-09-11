---
layout: default
title: 코딩-5
parent: 코딩 문제
---

<br>
출처: <https://leetcode.com/problems/longest-substring-without-repeating-characters/>

<br>
**문제 설명**
<br>

Given a string s, find the length of the longest substring without repeating characters.

<br>

**문제풀이**

```js
const solution = (s) => {
  let subStringLength = [0];
  let subString = "";

  for (let index = 0; index < s.length; index++) {
    for (let i = index; i < s.length; i++) {
      if (!subString.includes(s[i])) {
        subString += s[i];
      } else {
        break;
      }
    }

    subStringLength.push(subString.length);
    subString = "";
  }

  return Math.max(...subStringLength);
};
```
