---
title: cmd 대체 프로그램 ckw 설치 및 설정
date: 2015-01-13 17:19:22
categories:
- tools
tags:
- cmd
---

윈도우를 쓰다보면 필수적으로 쓸 수 밖에 없는 cmd.exe... 그런데 너무 구리다. 구린 것은 참고 써왔는데, 터미널에 보이는 글자들을 선택하여 복사하는 방법조차 간단하지 않다. 이것은 못 참겠다.

<!-- more -->

맥 터미널도 별로라고 생각되어 iTerm 을 써왔는데, cmd를 쓰다보면 맥 터미널이 아름답기까지 하다. cmd를 대체할 수 있는 도구는 없을까 고민하며 구글링 하던 중 괜찮은 프로그램을 찾았다.

이름하여 **ckw!!**

2007년 11월 달에 일본인이 개발한 것으로 보인다. 현재까지도 이것을 대체할만한 프로그램은 몇 없는 것 같다. 개발자의 [공식 블로그](http://d.hatena.ne.jp/hideden/20071115/1195229532)에 접속하여 다운받을 수 있고 아래 링크를 통해서도 가능하다.

[ckw-0.8.10 download](http://hideden.net/pub/ckw-0.8.10-bin.zip)

압축을 풀면 딱 세 개의 파일로 되어 있는데 ReadMe는 무시하고 필요한 파일은 실행파일인 `ckw.exe` 파일과 설정파일인 `ckw.cfg` 파일 두 개다. 실행파일을 실행해보면 바로 화면이 보이는데, 그닥 미려하진 않다. ~~난 무엇을 바랬던 것이냐...~~

dogfeet님이 소개하는 Solarized Theme를 적용하면 그나마 낫다. [바로가기](http://dogfeet.github.io/articles/2013/git-msysgit-ckw-solarized.html)

아래는 나의 스타일과 설정에 맞춘 설정파일이다. 시작 디렉토리인 chdir과 font 글꼴과 사이즈를 변경하였다. 자기 입맛에 맞추어 변경하여 사용하면 될 듯 하다.

```sh
!
! ckw setting
!

Ckw*title: Powershell
Ckw*exec:  powershell -ExecutionPolicy RemoteSigned
Ckw*chdir: D:\

Ckw*scrollHide:  no
Ckw*scrollRight: yes
Ckw*internalBorder: 1
Ckw*lineSpace: 1
Ckw*topmost: no

Ckw*font: Monaco
Ckw*fontSize: 14

Ckw*geometry:  80x50
Ckw*saveLines: 10000

!! theme
!! Solarized dark
!!

!Ckw*foreground:     #657b83
Ckw*background:     #073642
!Ckw*cursorColor:    #657b83
Ckw*cursorImeColor: #dc322f
!Ckw*backgroundBitmap: background.bmp
!Ckw*transp:           220
!Ckw*transpColor:      #000000

Ckw*color1:  #586e75
Ckw*color2:  #859900
Ckw*color3:  #2aa198
Ckw*color4:  #cb4b16
Ckw*color5:  #6c71c4
Ckw*color6:  #859900

Ckw*color8:  #839496
Ckw*color9:  #268bd2
Ckw*color10: #859900
Ckw*color11: #2aa198
Ckw*color12: #dc322f
Ckw*color13: #d33682
Ckw*color14: #b58900
Ckw*color15: #fdf6e3
```

설정한 파일을 `C:\windows\System32` 아래 넣어둔 후 윈도우 + R 키를 눌러 뜨는 실행창에서 `cmd` 대신 `ckw`를 입력하여 사용하면 된다.

뭔가 맥 터미널 느낌나고 전보다 휠씬 낫구나. 야호!

---

#### 참고
- http://s0und0ne.tistory.com/24
- http://dogfeet.github.io/articles/2013/git-msysgit-ckw-solarized.html
