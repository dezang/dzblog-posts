---
title: Atom 에디터에서 나만의 Snippet 만들기
categories: tools
tags: atom
---
오랜 기간동안 Sublime Text(이하 서브라임) 를 사용해오다가 얼마 전부터 Atom(이하 아톰)으로 갈아탔다. VSCode 중에 무엇을 주력 텍스트 에디터로 사용할지 고민하였으나 일단 아톰을 손에 익히기로 결정했다. Github 와의 더 깊은 유대감을 만드고 싶기도 했고 ~~응? 이게 무슨 상관?ㅋㅋㅋ~~

<!-- more -->

따라서 지금 작성되는 포스트 또한 아톰으로 작성되고 있다. 그런데 Hexo 내 포스트들은 YAML 형식으로 된 공통적으로 들어가야만 하는 부분이 있다.

```yaml
---
title: 효율적인 AWS 리전별 속도 측정
categories:
- aws
tags:
- latency
---
```

모든 글들은 위와 같은 데이터 블럭을 가져야 하는데, 터미널에서 `hexo new 'POST_TITLE'` 명령어를 치면 자동적으로 파일을 생성해주지만 보통 바로 아톰을 열어서 글을 작성하기 마련이다. 그러면 항상 저 공통 부분을 작성해야 하는데 이게 상당히 귀찮다. 이 귀차니즘을 해결해주는게 바로 스니펫 기능이다. 스니펫은 들어봤을거라고 생각하고 작성법부터 알아보자.

## 스니펫 설정 파일 열기
커스텀 스니펫 파일의 위치는 `~/.atom/snippets.cson` 이다. 이 파일을 수정해줘야 한다. 직접 경로에 들어가서 열어줘도 되지만 아톰에서 바로 열 수 있다. 메뉴바에 `Atom -> snippets...` 을 선택하면 된다. 그런데 이렇게 마우스 클릭질하자고 아톰쓰는거 아니자나? `Cmd + Shift + P` 를 눌러 커맨드 팔레트를 연다. `Application: Open Your Snippets` 를 입력하면 스니펫 설정 파일이 열린다.

잠깐 지금 저거 다 입력한 것은 아니겠지? 그건 커맨드 팔레트를 무시하는 행위다. 난 `app oys` 라고 입력하기로 정했다. 대략적인 알파벳을 순서에 맞게 입력해주면 해당하는 명령어 알아서 찾아준다. 이 맛에 서브라임이나 아톰 쓰는거 아니겠나. 파일을 열면 아래와 같은 문서가 보일 것이다.

```cson
# Your snippets
#
# Atom snippets allow you to enter a simple prefix in the editor and hit tab to
# expand the prefix into a larger code block with templated values.
#
# You can create a new snippet in this file by typing "snip" and then hitting
# tab.
#
# An example CoffeeScript snippet to expand log to console.log:
#
# '.source.coffee':
#   'Console log':
#     'prefix': 'log'
#     'body': 'console.log $1'
#
# Each scope (e.g. '.source.coffee' above) can only be declared once.
#
# This file uses CoffeeScript Object Notation (CSON).
# If you are unfamiliar with CSON, you can read more about it in the
# Atom Flight Manual:
# https://atom.io/docs/latest/using-atom-basic-customization#cson
```
자 이제 차근차근 이 문서를 수정해보자.

## 스니펫의 동작 범위, Scope
스니펫은 어디서나 동작하는게 아니라, 그 문서의 Syntax에 따라 동작한다. 주석처리되어 보여주는 위 예제의 경우 `.source.coffee:`라고 적힌 부분이 스코프의 선언이다. 따라서 저 스니펫은 CoffeeScript 문서에서만 사용할 수 있다. 문서의 형식은 아톰 하단에 적혀있다.

![문서형식확인](https://lh3.googleusercontent.com/ckaSBVO0lLLBidbjcLV36ub_k-31uAOitMUZnV5raRdSLeGlUyxSgHFYGCUqKma9lq3OU7GbLztUfCEI2ogy5iINIb7XcBdgOf9xRBpWUgjgNHscONg7z2B8LvK_PQGR74j0hAOShpIuDgbYZezrOlKhN1PYWTpTdwJgOvaBbbQrEozLaSJZg4r9NSdBjOWZ0iFDxbEN5XAnh2vP3CsNPT1bnSVvEtRrh0Jj6JzdM5XZDHiHO2zApdq9QgIOxTSlq2l7IPZmT8FR9bUCKKOR4qEWZdlHWQnAMIJSGls3gbGqZrzdQUxJa7d3K6TnhxhtlVlGvkuzw0lmajQ4IhNhKhoZ783Lh2YtmNsZAqYczl9BkLu2_T1jOC1PNDSkV60gl_4TwW9DQhhDZ-FHz6cT2HXA1CaIUG6h-m3C0u3IAY2ACMn6YIVFeU7NBO3K71FEavIMsaaROCoi5gg4yXzhBChiCboW7TwwWs7sSWqMf85j8FIllWA5y9EK4yQ0DBRzO3WUinwyJgKUhjrBup_2lfKbt-v-R9vhD-iFUY3hCUy8aEZ00RqEgmtOUSs4lcAwsbv5-YZKbf3TI66X1lBdbbCjBvB7Cb6a7dMpLm1aqYEwbx95=w605-h307-no)

내가 글을 작성하고 있는 이 문서는 `Github Markdown` 형식이라는 거다. 저기 누르면 언어를 선택할 수 있다. 그러면 선택한 언어로 문서를 인식한다. 보통 파일 확장자를 가지고 자동으로 어떤 문서인지 인지한다. 하지만 아직 파일명을 정하지 않았다면 저곳을 선택해 변경할 수 있다. 그렇다고 `.source.coffee:` 부분에 저기에 보이는 문자를 그대로 입력하면 안된다. 정확한 스코프명이 있다. 이는 커맨드 팔레트에 `Log Cursor Scope`라고 입력하거나 `Cmd + Opt + P`를 눌러 확인하면 된다. 그러면 에디터 우측 상단에 스코프명이 뜬다.

![스코프명 확인](https://lh3.googleusercontent.com/a6ZCFLiQrIHQPTirCWFOWBlqMl2-Z8evk3M6KZQWrFDP7Ap7W6_uqLP5Rp1EGtJswnL1hRifkjli2wqIw2hcgQfDsYIH1Mkwm7ML_dYhmZpLY828ErTPUL2xIW8xukFape83ATywjvTSXL5YxcXrA2xvjk7bfO-PG0uN03YJ9zgdCXECpZ5MPKjwiNUKOz8FQYXWoq0HtJDdFNlliWYDlHNObCWLAWxAyAGViVM4wKF3UU0JcTSqGT7s0B3sirhye3jwvf0sI4NviR4ncWAGXNr7cLu6jLkboiAiNvwWSk1SFKZojpHszuNDE9GCLEl9iHtdxfl8aCWZfKPGzv7wII3y0BBmiNfN2auhuDyDP3uW5S8eiqYOA1k2A1BRTRkwgLYYvmDwfGDLPYrcKxV3Ze0yHQoBvg5HsIDI8ald1XeVUM-FU88r9f5G32ja9f6pCDwbHkUUnZVSy7vD5XPRo0Xx5MPWX0myIa8YdNYdnqX2jmeszydvrRDSGf83ybzI5jqLr_gbYyK-HiINdWzzn9zW5tf1-pqcH-FxGNL_Pp7rNW-DV_69U3lqg0hy2d9uQLOg_HlBMBrUNjpMYMbEZF_ITeB7pi2th2CrRQUt0ax8mSXB=w571-h219-no)

만약 다른 문서 형식에 스코프를 만들겠다면 위에 설명한 방식으로 확인하면 된다. 잠깐, 문서 형식을 변경할 때 또 마우스 커서를 움직이려고 하진 않았겠지? 단축키는 `Ctrl + Shift + L` 이다. 자주 쓰일 단축키니 숙지하자.

## 본격적인 스니펫 작성
아까 그 예제를 다시 한번 살펴보자.

```cson
# '.source.coffee':
#   'Console log':
#     'prefix': 'log'
#     'body': 'console.log $1'
```

첫번째 줄은 작성하는 스니펫이 동작할 스코프를 선언하는 부분이라는 것은 앞서 말했고, 그 다음은 스티펫의 이름이다. 알아보기 쉽게 적도록 하자. 나는 새로운 포스트를 만들 때 사용할 스니펫이 필요하므로 `New Post`로 정했다.

세번째 줄은 **prefix** 로 스니펫을 불러올 축약어인데 이는 사용하고 쉽도록 정하자. 그렇다고 너무 짧게 정해버리면 무심코 누른 Tab에 스니펫이 불러와지는 불상사가 일어날 수 있으니 조심하자. 나는 `npost`로 정했다.

네번째 줄부터 본격적인 **body** 가 시작된다. 축약어를 입력하고 탭을 눌렀을 때 입력된 문자들을 입력하면 된다. 한 줄의 경우 위 예제와 같이 싱글쿼터로 감싸주면 되는데, 여러 줄일 경우 싱글쿼터 세개로 감싸줘야 한다.

```
"""
요 안에 여러줄 문자 넣기!
"""
```

또한 `$1`,`$2` 로 위치를 지정하면 문자 입력 후 탭을 누르면 다음 번호 위치로 자동으로 이동한다. `$1:PlaceHolder` 처럼 작성할 수도 있다. 아래는 이러한 작성 규칙을 지키며 내가 작성한 스니펫이다.

```cson
'.source.gfm':
  'New Post':
    'prefix': 'npost'
    'body': """
    ---
    title: ${1:title}
    categories: $2
    tags: $3
    ---

    <!-- more -->

    ----
    #### 참고
    """
```

## 스니펫 사용
나만의 스니펫을 사용하기 위한 모든 설정이 끝났다. 이제 마크다운 문서에서 `npost`를 입력 후 탭을 누르면 내가 지금까지 지겹게 타이핑 해온 포스트 정보 섹션을 자동으로 넣어준다. 아 아름답도다.

기본적으로 제공해주는 스니펫도 상당히 많다. 이는 커맨드 팔레트에서 알아서 검색해보도록. 각 언어별로 작성된 스니펫 파일은 [아톰 Github](https://github.com/atom)에서 확인할 수 있다. 여기 html 스니펫 파일을 [링크](https://github.com/atom/language-html/blob/master/snippets/language-html.cson)해둔다. 어떻게 스니펫을 작성하고 관리해야할지 감이 올 것이다.

## 마치며
스니펫 내용뿐만 아니라 아톰의 기능도 틈틈히 설명하다보니 글이 길어졌지만 스니펫의 작성과 사용법은 무척 간단하다. 계속적으로 반복하는 코드나 문구가 있다면 조금 시간을 내서 스니펫을 작성하자. 스니펫은 의미없는 타이핑을 해야만 했던 당신의 손가락에 휴식을 가져다 줄 것이다.

----
#### 참고
- http://flight-manual.atom.io/using-atom/sections/snippets/
- http://www.hongkiat.com/blog/add-custom-code-snippets-atom/
