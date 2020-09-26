---
layout: default
title: Jekyll
parent: 기타
---

**현상**

실행 시 경고문구가 발생한다.

```shell
$ bundle exec jekyll serve

...

# 경고
GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.

...

```

<br>
**해결**

- GitHub에 personal access token 생성 [(참고)](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token)

- 환경변수에 token 추가

```shell
export JEKYLL_GITHUB_TOKEN=[personal access token]
```

- 재실행 하면 경고없이 시작된다.
