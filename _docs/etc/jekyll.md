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

---

**현상**
```shell
...
# 에러
listen-3.2.1 requires ruby version >= 2.2.7, ~> 2.2, which is incompatible with the current version, ruby 3.0.1p64

...
```

<br>
**해결**
- bundle update listen 실행

---

**현상**
```shell
...
# 에러
bundler: failed to load command: jekyll (/usr/local/lib/ruby/gems/3.0.0/bin/jekyll)
/usr/local/lib/ruby/gems/3.0.0/gems/jekyll-3.9.0/lib/jekyll/commands/serve/servlet.rb:3:in 'require': cannot load such file -- webrick (LoadError)

...
```

<br>
**해결**
<br>
webrick 추가
- bundle add webrick

