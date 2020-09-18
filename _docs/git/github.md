---
layout: default
title: GitHub
parent: Git
---

# **GitHub**

---

## Flow

![](../../../images/git/github-flow.png)

이미지 출처 : <https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf>

<br>

### Branch 생성

master branch 는 항상 배포가 가능해야 하기에 새로운 작업에 대한 새 브랜치를 생성한다.<br>
작업하는 내용에 대해 브랜치의 이름으로 작성하기를 권장한다. 팀원 간 어떤 작업 중인지 파악이 용이하다. <br>
ex) authentication, make-avatar ...

<br>

### Commit 추가

생성한 브랜치에서 작업을 시작하며 커밋을 추가해 간다.<br>
작업의 기록이기에 커밋메시지는 작업에 대한 설명으로 작성하기를 권장한다.

<br>

### Pull request 및 review

작업 후 커밋을 푸시하게 되면 해당 브랜치에 대한 풀리퀘스트 버튼이 활성화된다.(기본 풀리퀘스트 버튼으로도 가능)<br>
풀 리퀘스트는 마스터 브랜치로 병합되기 전에 의견을 나눌 수 있도록 한다.
리뷰 등을 통해 변경 작업을 진행하고 커밋을 추가한다.

**리뷰 방법**
<br>

- PR에 Files changed 탭을 선택한다.

  ![](../../../images/git/pr-files-changed.png)

- 변경된 파일의 내용 중 리뷰하고자 하는 줄의 플러스 아이콘을 선택한다.

  ![](../../../images/git/pr-select.png)

- 리뷰 내용을 적고 start a review 버튼을 클릭한다.

  ![](../../../images/git/pr-review-write.png)

- 우측 상단의 review changes 버튼이 finish your review로 바뀐다. 해당 버튼을 누르고 제출하면 된다.
  ![](../../../images/git/pr-finish.png)
- 그럼 PR의 conversation 탭의 아래부분에 리뷰가 나타난다.

  ![](../../../images/git/pr-review.png)

<br>

### Merge

프로젝트 관리자의 검토가 완료되면 마스터브랜치와 병합한다.<br>
이때 closes #issue번호를 적으면 병합과 함께 해당 이슈도 닫힌다.
만약, 풀리퀘스트 생성 시 linked issues를 통해 이슈를 연결해 놓았다면 closes #issue번호가 없어도 자동으로 닫힌다.
