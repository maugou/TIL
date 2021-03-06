---
layout: default
title: 파일 권한
parent: 기타
---

```shell
$ ls -la
drwx------      ...
drwxr-xr-x      ...
drwxr-xr-x      ...
-rw-r--r--      ...
```

**권한 읽는 법**

'-rw-r-x-w-' 중 가장 앞 자리는 'd'일 경우 폴더이며, '-'는 단일 파일이다.<br />
나머지 9자리는 3자리씩 나누어서 첫번째는 접속한 계정소유자를 의미한다.
두번째는 그룹을 의미하며, 세번째는 소유자, 그룹을 제외한 사람들을 의미한다.

r은 read, w는 write, x=excute(실행)의 의미이다.<br/>
숫자로는 r=4, w=2, x=1을 의미한다. 세자리의 합산으로 전체 권한의 수를 알수 있다. 예를 들면, '-r----x-w-'는 412 이다

**권한 부여하기**

숫자로 권한 제어
```shell
$ chmod 412 [파일명]

-r----x-w-  ... [파일명]
```

모든 사용자의 권한 제어<br/>
+는 권한 추가, -는 권한 삭제<br/>

```shell
$ chmod +x [파일명]

-r-x--x-wx ... [파일명]
```
