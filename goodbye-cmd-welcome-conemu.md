---
title: cmd는 버리자. windows console의 결정판 conemu
date: 2015-01-20 17:24:22
categories:
- tools
tags:
- cmd
---

나는 우분투와 맥이 주력 OS이지만, 사내 작업 환경은 오로지 windows다. 멀티 플랫폼을 지원하는 이클립스 덕분에 IDE 자체는 크게 다르지 않아 개발에는 큰 문제가 없었지만, 단 하나 참을 수 없는 것은 윈도우의 기본 터미널(console) 도구 cmd다.

<!-- more -->

이미 이전에 `ckw`라는 프로그램에 powershell을 조합해서 그나마 나은 환경을 만드는 방법 ({% post_link ckw-cmd-alternative %})을 소개했지만 ckw는 나를 만족시키지 못했다. ~~그래도 그 당시에는 이게 최선이라고 생각했다.~~

그리고 conemu를 접하고 난 지금. 앞으로 윈도우 터미널 작업에 더 이상 불만을 갖지 않을 것이다. 그 동안 이 툴을 왜 모르고 살았을까 싶을 정도다. conemu는 cmd, powershell 같은 터미널 도구들 뿐만 아니라 chrome이나 sublime 조차도 탭 컨텍스트로 포함 가능한 도구다.

conemu에 git bash 이나 cygwin을 띄운다면 리눅스에서 느꼈던 그 사용자 경험을 그대로 윈도우에서 느낄 수 있다. 설정의 범위도 상당해서 자신의 입맛대로 커스터마이징이 가능하다. 백문이 불여일견. 장점을 주절주절 설명하기보다는 사진 한 장으로 그 모습을 보여드리도록 하겠다.

![conemu 최종 설정 스샷](https://monosnap.com/image/kCfQUxl1NUPeAGsDlRG2QrDbtUPY8q.png)

어떠한가? multi tab, split, color theme, default dir config 없는게 없다. 일단 [conemu 공식 사이트](https://code.google.com/p/conemu-maximus5/)에서 다운로드 받아 설치하도록 하자. portable을 지원하니 dropbox 같은 클라우드에 올려놓으면 어디서든 동일한 환경의 console 도구를 사용할 수 있다!

해당 프로그램을 조금 변경한 [cmder](https://github.com/bliker/cmder)라는 프로젝트도 있긴한데, 구지 저걸 쓰지 않더라도 설정만 잘 맞춰주면 똑같이 쓸 수 있더라. 컬러 테마 설정이 없는줄알고 저 프로젝트 쓸 뻔했다.

win + alt + p 키룰 눌러 환경설정 창을 띄우고 하나하나 자신에 맞는 환경을 설정한다. 세부적인 conemu에 설정 방법은 추후를 기약하고,~~굳이 적을 필요가 있을까 싶기도;;~~  git bash와 Consolars가 가진 한글 문제를 해결하는 방법을 적어보겠다.

## git bash 한글 물음표 해결
```sh
$ vim ~/.inputrc
```

```
set output-meta on
set convert-meta off
```

이것으로 콘솔에서 한글 입출력은 해결 했지만 ls 의 한글 출력은 여전히 물음표다. 이를 해결하기 위해 아래 과정을 진행한다.

```sh
$ vim ~/.bashrc
```

```
alias ls = "ls --show-control-chars"
```

이렇게 하고나서 `source ~/.bashrc` 를 입력하거나 터미널을 재시작하면 한글은 보인다. 다만 별로 진지하지도 않으면서 궁서체같은 폰트가 거슬린다. 이는 consolars가 영문만 지원하기 때문이다.

## 궁서체를 맑은 고딕으로!
이미 이에 불만을 느낀 능력자들이 consolars에 맑은고딕을 합쳐 놓았다. 폰트를 다운받아 설치하면 맑고 아름다운(?) 한글이 당신을 반길 것이다.

대장은 맑은고딕이 합쳐진 monaco를 사용해왔었는데, 이제보니 consolars가 monaco보다 이쁜 것 같다. 여기 두 폰트 모두 링크를 걸어두었으니 자신에게 맞는 폰트를 설치하여 사용하자.

###### 폰트 다운로드
- [Monaco + 맑은고딕.ttf](https://www.dropbox.com/s/3dchb7v4e3datty/Monaco%20%EB%A7%91%EC%9D%80%20%EA%B3%A0%EB%94%95.ttf?dl=0)
- [Consolars + 맑은고딕.zip](https://www.dropbox.com/s/9voqp3yowtmrfz3/Consolas_%EB%A7%91%EC%9D%80%EA%B3%A0%EB%94%95.zip?dl=0)

이 정도 설정만 하면 윈도우에서 그리웠던 리눅스/mac 의 터미널을 사용할 수 있을 것이다. ls 대신 dir 에 적응해야 했던 윈도우에서 눈물을 머금고 작업해 온 개발자들이여. 이제 cmd를 버리고 conemu + git bash 조합으로 활짝 웃어보자!

---

#### 참고
- [conemu 공식 사이트](https://code.google.com/p/conemu-maximus5/)
- [conemu wiki](https://code.google.com/p/conemu-maximus5/wiki/TableOfContents?tm=6)
- http://codeheart.tistory.com/121
- http://imjuni.tistory.com/606
- http://www.hanselman.com/blog/ConEmuTheWindowsTerminalConsolePromptWeveBeenWaitingFor.aspx
- http://czstyle.tistory.com/29
