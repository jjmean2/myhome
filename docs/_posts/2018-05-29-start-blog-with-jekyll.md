---
layout: post
title:  Jekyll로 블로그 만들기
date:   2018-05-29 03:03:06 +0900
categories: jekyll update
tags: blog jekyll
---

깃허브에 Jekyll을 지원한다고 한다. Push만 해도 Publishing을 한다고 하는데 무슨 뜻일까?

일단 빠르게 jekyll 사이트를 새로 만드는 과정을 보자.

```sh
$ gem install jekyll bundler
```

RubyGem `gem` 명령을 사용하여 `jekyll`과 `bunlder` gem을 설치한다. gem은 라이브러리 같은 것이다.
`jekyll`은 Jekyll 정적 사이트 생성기 라이브러리를 설치하는 것이고 `bundler`는 gem의 의존성 관리 라이브러리이다. bundler가 하는 일은 모든 gem이 global하게 설치되는 Ruby환경에서 각 프로그램마다 필요로 하는 버전의 gem을 고를 수 있도록 하는 것으로 생각된다.

```sh
$ jekyll new blog
```

위와 같이 명령하면 현재 디렉토리에 `blog`라는 이름의 디렉토리가 생기고 거기에 jekyll의 기본파일들이 생성된다. 그 파일의 목록은 다음과 같다.

```sh
.
├── .gitignore
├── 404.html
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2018-05-26-welcome-to-jekyll.markdown
├── about.md
└── index.md

1 directory, 8 files

```

참고로 `.gitignore`의 내용은 다음과 같다.

```sh
_site
.sass-cache
.jekyll-metadata

```

위의 git에 의해 ignored 되는 파일들은 최초 생성된 파일 중에 하나도 없는데 설명하면 다음과 같다.

* `_site`: 생성된 static site 디렉토리, 이것은 jekyll을 빌드함으로써 생성되는 디렉토리로 이 안에는 `index.html`로 시작하는 publish할 웹사이트 자체가 들어있다.
* `.sass-cache`
* `.jekyll-metadata`

위의 파일들을 무시한다는 것은 github(git 서버)에는 빌드 결과이자 생성할 사이트 자체인 `_site`는 푸시하지 않겠다는 것이다. 이 디렉토리가 웹서버에 있어야 사이트를 볼 수 있다. 예컨대 jekyll 같은 도구를 사용하지 않고 github pages를 사용할 때는 page로 사용할 브랜치의 root가 웹사이트 디렉토리가 되고 거기에 `index.html`이 있어야 접속했을 때 페이지가 보이게 된다.

실제로 깃허브에 푸시된 파일은 다음과 같다.

```sh
$ git ls-files
.gitignore
404.html
Gemfile
Gemfile.lock
_config.yml
_posts/2018-03-03-welcome-to-jekyll.markdown
about.md
index.md

```

하지만 이 상태로 github-pages 주소인 `jjmean2.github.io`에 접속해보면 jekyll blog 사이트가 떴다. 즉, build와 serve 부분은 깃허브가 알아서 해주는 것이다.

로컬에서 jekyll 사이트를 띄워보려면 다음과 같이 build-serve 과정을 거쳐야 한다. serve를 하면 build가 안 된 경우 알아서 build를 한다.

```sh
$ bundle exec jekyll build
$ bundle exec jekyll serve
```

다음은 빌드(`bundle exec jekyll build`)를 실행했을 때 콘솔 출력과 그 이후 파일트리이다.

build 실행
```sh
$ bundle exec jekyll build
Configuration file: /Users/ljw/dev/playground/terminal/jekyll-blogs/blog/_config.yml
            Source: /Users/ljw/dev/playground/terminal/jekyll-blogs/blog
       Destination: /Users/ljw/dev/playground/terminal/jekyll-blogs/blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.568 seconds.
 Auto-regeneration: disabled. Use --watch to enable.
```

파일트리
```sh
$ tree .
.
├── .gitignore
├── .sass-cache
│   ├── 1df6e6ceddab419841c69a78595245fd99225f1c
│   │   └── minima.scssc
│   └── 5116d62066105af7e5a1d60e7f790603b407cb6c
│       ├── _base.scssc
│       ├── _layout.scssc
│       └── _syntax-highlighting.scssc
├── 404.html
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2018-05-26-welcome-to-jekyll.markdown
├── _site
│   ├── 404.html
│   ├── about
│   │   └── index.html
│   ├── assets
│   │   ├── main.css
│   │   └── minima-social-icons.svg
│   ├── feed.xml
│   ├── index.html
│   └── jekyll
│       └── update
│           └── 2018
│               └── 05
│                   └── 26
│                       └── welcome-to-jekyll.html
├── about.md
└── index.md

12 directories, 19 files

```

콘솔 로그를 분석해보면,
* `_config.yml` 을 설정파일로 쓰고
* 현재 폴더를 소스로 쓰고
* `_site` 디렉토리에 결과 웹사이트를 넣는다는 뜻이다.

원래 파일에 더해 `_site` 디렉토리와 `.sass-cache` 디렉토리가 추가되었음을 알 수 있는데 둘다 `.gitignore`에 포함되어 있어서 커밋되지는 않을 빌드 결과물이다.


# 참고자료
* <https://xho95.github.io/blog/github/pages/jekyll/minima/theme/2017/03/04/Jekyll-Blog-with-Minima.html>