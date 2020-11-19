---
layout: default
title: Git
has_children: true
has_toc: false
---

# Git
{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Flow

![](../../../images/git/git-flow.png)

- master : 배포되었거나 배포할 브랜치
- develop : 다음 버전을 개발하는 브랜치
- feature : 기능을 개발하는 브랜치
- release : develop브랜치의 작업 내용이 정식버전으로 병합 전에 분기되는 브랜치 (즉, master로 병합 전 테스트하는 브랜치)
- hotfix : 배포 버전에서 발생한 버그를 수정하는 브랜치

master와 develop 브랜치가 주요 브랜치이면 나머지 브랜치는 운영에 따라 선택되는 브랜치이다.

## Git 저장소에 따른 workflow

upstream과 origin remote 저장소가 있으면 local에서 코드 수정하고 origin에 푸시 후 upstream으로 MR을 한다.<br>
이후 병합된다면 upstream에 있는 코드를 풀하여 사용한다. (flow만 참고)

## 명령어

- git config --list : git 설정 정보 보기
- git fetch --all --prune : remote branch에서 brach의 삭제나 추가가 있을 시 local에 동기화 시키는 작업
- git clone [git URL]: 저장소 가져오기 (저장소 명으로 폴더가 생성되고 그 안에 파일들이 위치)
- git branch : 현재 사용 중인 로컬 branch 확인
- git branch [branchName] : branch 생성
- git checkout [branchName] : branch 변경

...



## 에러 대응

### 로컬과 원격 저장소의 기록 시작이 다를 경우

- **현상**<br/>
git pull이 되지 않는다.(refusing to merge unrelated histories)

- **해결**<br/>
병합을 통해 pull이 되도록 한다.

```shell
$ git pull origin 브런치명 --allow-unrelated-histories
```

<br/>

### SSH keys 파일권한
- **현상**<br/>
권한 문제로 git과 연결되지 않는다.

```shell
$ git clone [git주소]

...
Permissions 0466 for '/Users/maugou/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/Users/maugou/.ssh/id_rsa": bad permissions
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
...

```  
- **해결**<br/>
사용자에게만 읽기 권한을 부여한다.

```shell
$ chmod 400 id_rsa
```
