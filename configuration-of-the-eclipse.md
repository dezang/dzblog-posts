---
title: 이클립스 / STS 설정의 모든 것
date: 2015-04-14 16:56:47
categories:
- tools
tags:
- eclipse
---

이클립스는 개발자에게 없어서는 안되는 필수 도구이다. 자바 개발자라면 대부분 이클립스를 사용하고 있을텐데, 이클립스를 자신의 환경에 맞게 설정한 후 사용하면 개발이 더 편해진다.

<!-- more -->

대장 또한 컴퓨터를 갈아 엎고나서 이클립스를 설치 한 뒤 가장 먼저하는 것이 환경 설정이다. 매번 이곳저것 뒤져가며 설정하는 것이 번거로워 이클립스 설정을 한자리에 모두 모아보았다.

대장이 꺼려하는 것 중에 하나는 지금하는 설정이 무엇을 변경하는 것인지도 모른 채 설정하는 것이다. 따라서 각 설정마다 최대한 자세히 설명을 첨부하려고 노력했으며, 설명을 보고 자신에게 필요없는 설정은 하지 않으면 된다. 혹시라도 설정하다가 모르는 부분을 댓글로 문의한다면 따로 자세히 포스팅하도록 하겠다.

*(추가적으로 이클립스 성능향상이나 개발자에게 도움이 되는 설정이 있으면 댓글로 알려주세요!)*

이클립스를 사용하는 개발자들에게 많은 도움이 되었으면 한다.

## 이클립스 STS ini 설정 파일 최적화

아래는 변경하지 않은 이클립스/STS 설정 파일의 전문이다. 혹시라도 설정을 변경하다가 실행이 되지 않거나 오류가 발생한다면, 아래를 참고하여 기본 설정값으로 되돌리면 된다.

### ini Default(기본) 설정
#### 이클립스 기본 설정(eclipse.ini 수정)

```
    -startup
    plugins/org.eclipse.equinox.launcher_1.3.0.v20130327-1440.jar
    --launcher.library
    plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.200.v20140116-2212
    -product
    org.eclipse.epp.package.jee.product
    --launcher.defaultAction
    openFile
    --launcher.XXMaxPermSize
    256M
    -showsplash
    org.eclipse.platform
    --launcher.XXMaxPermSize
    256m
    --launcher.defaultAction
    openFile
    --launcher.appendVmargs
    -vmargs
    -Dosgi.requiredJavaVersion=1.6
    -Xms40m
    -Xmx512m
```

#### STS 기본 설정(STS.ini 수정)
```
    plugins/org.eclipse.equinox.launcher_1.3.0.v20120522-1813.jar
    --launcher.library
    plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.200.v20120913-144807
    -product
    org.springsource.sts.ide
    --launcher.defaultAction
    openFile
    --launcher.XXMaxPermSize
    256M
    -vmargs
    -Dosgi.requiredJavaVersion=1.6
    -Xms40m
    -Xmx768m
    -XX:MaxPermSize=256m
    -Dgrails.console.enable.interactive=false
    -Dgrails.console.enable.terminal=false
    -Djline.terminal=jline.UnsupportedTerminal
    -Dgrails.console.class=grails.build.logging.GrailsEclipseConsole
```
### ini 파일 변경

위 파일에서 `vmargs` 이하 부분을 아래와 같이 변경 혹은 추가한다. 각 설정에 따른 설명은 아래 쪽에 기재하였다.
```
    -vmargs
    -Dosgi.requiredJavaVersion=1.6
    -Xverify:none
    -XX:+UseParallelGC
    -XX:+AggressiveOpts
    -XX:-UseConcMarkSweepGC
    -XX:PermSize=128M
    -XX:MaxPermSize=128M
    -XX:MaxNewSize=128M
    -XX:NewSize=128M
    -Xms512m
    -Xmx512m
```

### ini 변경 사항 상세 설명

Dosgi.requiredJavaVersion=1.6
:   JDK 1.6 이상을 설치했을 경우에 1.6으로 설정하면 속도가 빨라진다.

Xverify:none
:   클래스의 유효성을 검사 생략. (시작 시간이 줄어 빨라진다.) 초기 시동시 verfify체크를 하지 않는다. 당연히 시동이 빨라진다. 플러그인의 features에 문제가 발생 할 수 있는데 플러그인에 변경 사항이 있을 경우에는 이걸 키고 시동하고, 별 문제 없으면 추가해서 사용한다.

XX:+UseParallelGC
:   병렬 가비지 컬렉션 사용. (병렬 처리로 속도 향상) Parallel Collector를 사용 하도록 한다. 체감 속도가 올라간다. 다중 프로세서를 사용한다면 필수.

XX:+AggressiveOpts
:   컴파일러의 소수점 최적화 기능을 작동시켜 빨라진다.

XX:-UseConcMarkSweepGC
:   병행 mark-sweep GC 수행하여 이클립스 GUI의 응답을 빠르게한다.

XX:+CMSIncrementalMode=true
:   점진적인 GC

XX:PermSize=128M
:   Permanent Generation(영구 영역) 크기(Out Of Memory 에러시 크기 조절)

XX:MaxPermSize=128M
:   최대 Permanent Generation 크기

XX:NewSize=128M
:   New Generation(새 영역) 크기

XX:MaxNewSize=128M
:   New Generation(새 영역) 의 최대 크기

Xms512m
:   이클립스가 사용하는 최소 Heap 메모리

Xmx512m
:   이클립스가 사용하는 최대 Heap 메모리 최소와 최대를 같은 값으로 설정하면 오르락 내리락 하지않아 빨라진다.

>[메모리 정의 예]
>1기가 이하 메모리인 컴퓨터인 경우 : -Xms256m -Xmx256m
>2기가 ~ 3기가 메모리인 컴퓨터 : -Xms512m -Xmx512m
>4기가 이상 메모리인 컴퓨터 : -Xms1024m -Xmx1024m

#### 메모리 설명
JVM 은 3가지 메모리 영역을 관리한다.

1. Permanent(영구) 영역 : JVM 클래스와 메소드를 위한 공간. = PermSize 설정
2. New/Young 영역 : 새로 생성된 개체들을 위한 공간. = NewSize 설정
3. Old 영역 : 만들어진지 오래된 객체들의 공간.(New 영역에서 이동해 온다)

## 필수설정
### 파일 인코딩 변경

- `Window -> Preferences -> General -> Content Types` 에서 각종 파일들의 인코딩을 UTF-8로 변경
- 메뉴에서 window-preference 선택 후 `General->Workspace` 에서 Text file encoding을 UTF-8로 변경

## 기타
### 컬러 테마 변경
- eclipsecolorthemes.org

### 이클립스 실행시 이미지변경
ini 파일에서 아래 항목을 추가한다. 이미지 경로와 파일명은 자신이 원하는 것으로 변경한다.

```
    - showsplash
    image/tan_helios.bmp
```

### 메모리 보기

`Window > Perference > General` 에서 Show heap status 체크하면 오른쪽 하단에 메모리를 볼 수 있다. 사용하다가, 메모리가 많이 올라 갔다 싶거나, 느려졌다 싶을 때마다, 옆에 쓰레기통 아이콘을 눌러서 메모리를 줄인다. 그러면 좀 더 쾌적하게 작업을 할 수 있다.

자동 가비지 컬렉션 플러그인 : http://gyuha.tistory.com/290

### 사용하지 않는 검사 제거

필요 이상으로 많은 검사를 많이 해서 느려지는 경우도 많다. HTML이나 그 외 등등의 검사도 거기에 포함된다. 딱 필요한 검사만 하도록 한다.
`Window > Perferences > Validation` 에서 자기가 사용하는 옵션만 켜둔다.

### 영어스펠링 검사 끄기
`Window > Prereces > General > Editors > Spellings에서 Enable spell checking`

### 기능 Disable / 습관 바꾸기
코딩하는 공간에서 잘 사용하지 않거나, 있는둥 마는둥 하는 기능을 꺼준다.

#### Automatic folding 끄기
코드 옆에 더하기 표시 나와서, 코드를 펼쳤다.. 닫았다 하는 기능이다.
`Window->Preferences->Java(또는 사용언어)->Editor->Folding` 모든 옵션을 해제(disable)한다.

#### Automatic Code Insight 끄기
`Window->Preferences->Java(또는 해당언어)->Editor->Code Assist` 에서 `Enable auto activation` 항목을 해제(disable)한다. 자동으로 동작하는 code assist 기능은 꺼지지만, ctrl+space으로 여전히 code assist를 사용할 수 있다. 손이 좀 불편하면 이클립스가 빨라진다. 하지만 대장은 이거 없인 못 살아서 켜놓았다.

#### 사용하지 않는 플러그인 삭제 하기
이클립스를 패키지로 설치하다 보면, 사용하지 않는 기능도 많이 들어 가게 된다. 그 중에서 사용하지 않는 플러그인은 삭제하도록 한다.

#### 사용하지 않는 프로젝트 닫아주기
현재 작업과 관련없는 프로젝트를 닫자. 이클립스가 접근하는 파일의 갯수를 줄여준다.

#### 사용하지 않는 파일은 닫아주기
작업하다가 사용하지 않는 창은 닫는다. 메모리가 절약된다. 이클립스를 종료시 편집하던 문서를 모두 닫고 종료하면 다음에 이클립스를 띄울때 좀 더 가볍게 띄울 수 있다. 그리고 `Window->Preferences->General->Editors`에서 `Close editors automatically`를 켜준다. 그럼 아래 설정된 숫자만큼만 문서가 열린다. 그 이상의 문서는 자동으로 닫아진다. 이렇게 사용하면, 무심결에 많이 열린 파일이 적어진다. 메모리 절약은 보너스!

#### 사용하지 않는 플러그인을 start list에서 제외하기
`Window > Preferences > General > Startup and Shutdown`에서, 불필요한 플러그인을 startup list에서 제외한다. 이렇게 하면 이클립스 실행시 좀 더 가볍게 실행 할 수 있다.

### 자동줄바꿈(word wrap)
- http://jedagi.tistory.com/30


---

#### 참고
- http://blackun.egloos.com/5179984
- http://ahtik.com/eclipse-update/
- http://kwonnam.pe.kr/wiki/eclipse/config
- http://blog.naver.com/sungback/90139189173
- http://blog.naver.com/PostView.nhn?blogId=sungback&logNo=90097516641
- http://thdnf1004.tistory.com/8
- http://yun0223.tistory.com/42
- http://gyuha.tistory.com/290
- http://blog.bagesoft.com/443
- http://www.slipp.net/wiki/pages/viewpage.action?pageId=5177633
- http://kwonnam.pe.kr/wiki/eclipse/config
- http://blog.naver.com/sungback/90139189173
- http://eclipsecolorthemes.org/
- http://fordev.tistory.com/79
- http://dkatlf900.tistory.com/55
