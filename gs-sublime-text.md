---
title: 서브라임 시작하기
categories:
- tools
tags:
- sublime
---

서브라임은 개발자를 위한 잘 만들어진 에디터다. 서브라임으로 프로젝트 관리하는 방법과 `Package Control`을 포함한 몇 가지 유용한 플러그인들을 소개하고, 그 사용법을 알아본다.

<!-- excerpt -->

## 프로젝트 관리
먼저 프로젝트 폴더를 서브라임으로 연다. 서브라임으로 프로젝트 폴더를 열었으면, 메뉴에서 `Project -> Save Project As...`를 눌러 현재 프로젝트를 저장한다.

같은 폴더에 원하는 프로젝트 이름을 입력하여 확장자가 `sublime-project`인 파일을 만든다. 그러면 두 가지 새로운 파일이 생긴다. `sublime-project`와 `sublime-workspace` 파일이다. 생성된 프로젝트 파일을 열어보면 다음과 같이 작성되어 있다.

```
{
  "folders":
  [
    {
      "follow_symlinks": true,
      "path": ".",
    }
  ]
}
```

path 에 경로를 바꾸면 바뀐 경로대로 사이드바에 파일들이 즉시 업데이트 된다. 경로는 프로젝트 파일(`example.sublime-project`)의 위치를 중심으로 상대 경로로 입력해도 되고, 절대 경로로 입력해도 된다. 프로젝트 폴더를 다른 위치로 옮길 경우 상관없도록 하기 위해서 프로젝트 파일을 프로젝트 폴더 최상위에 두고 상대경로를 입력하여 설정하도록 한다.

경로는 복수로 입력가능하다. 등록한 경로의 수만큼 폴더가 최상위로 등록된다. 아래는 현재 나의 대장넷 프로젝트 파일의 설정이다. 포스트에 더 접근하기 쉽도록 따로 빼놓았다.

```
{
  "folders":
  [
    {
      "follow_symlinks": true,
      "path": ".",
    },
    {
      "follow_symlinks": true,
      "path": "src/documents/posts"
    }
  ]
}
```

이렇게 설정해놓으면 나중에 프로젝트를 닫더라도 `Project -> Open Recent`에서 손쉽게 다시 설정한대로 열어서 작업할 수 있다. 여러 개의 프로젝트를 수행하다보면 프로젝트간의 이동이 중요하다. `control+command+P` 단축키로 프로젝트간의 이동을 빠르게 할 수 있다.

나중에 프로젝트 파일을 변경하고 싶다면 프로젝트 파일을 찾아 열어서 수정할 수도 있지만 커맨드 팔레트를 이용하면 더 쉽다. 커맨드 팔레트를 열고 `project`라고 입력하면 몇 가지 기능들이 보여지는데 `Edit`를 누르면 바로 프로젝트 파일이 열린다. 그 밖에도 `Add Folder`는 프로젝트 파일에 손쉽게 경로를 추가하는 좋은 방법이다.

프로젝트를 수행하다보면 프로젝트가 실행되기 위해서는 필요하지만 내가 굳이 변경하거나 열어보지 않아도 되는 파일이나 폴더들이 존재한다. 이러한 파일/폴더를 서브라임 작업환경에서 보이지 않게 설정이 가능하다.

먼저 **특정 확장자** 를 가진 파일들을 베제시켜 보도록 한다.

```
{
  "folders":
  [
    {
      "follow_symlinks": true,
      "path": ".",
      "file_exclude_patterns": ["*.sublime-project"]
    }
  ]
}
```

`file_exclude_patterns`를 입력하고 `[]` 안에 배제할 확장자를 넣어주면된다. 콤마를 사용하여 여러 개를 입력해도 좋다. 이와 마찬가지 방법으로 `folder_exclude_patterns`를 사용하여 폴더를 배제시킬 수 있다.

```
{
  "folders":
  [
    {
      "follow_symlinks": true,
      "path": ".",
      "file_exclude_patterns": ["*.sublime-project"],
      "folder_exclude_patterns": ["out","node_modules"]
    }
  ]
}
```

마지막으로 `setting`을 프로젝트에 한하여 적용하는 방법을 알아보자.

```
{
  "folders":
  [
    {
      "follow_symlinks": true,
      "path": ".",
      "file_exclude_patterns": ["*.sublime-project"],
      "folder_exclude_patterns": ["out","node_modules"]
    },
    {
      "follow_symlinks": true,
      "path": "src/documents/posts"
    }
  ],
  "settings":
  {
    "tab_size": 2
  }
}
```

위와 같이 설정하면 기본 세팅을 무시하고 해당 프로젝트 안에서는 설정된 세팅값으로 세팅된다. 프로젝트에 맞는 설정값을 따로 부여할 수 있는 것이다. 변경할 수 있는 세팅값들은 메뉴에 `Sublime Text -> Preferences -> Settings-Default`에서 확인 할 수 있다.

## 플러그인
서브라임은 강력한 텍스트 에디터다. 깔끔한 인터페이스와 강력한 기능에 매료된 많은 개발자들이 `TextMate`에서 서브라임으로 갈아탔다. 기본적인 기능만으로도 사용하는 것에는 문제가 없지만, 기능을 더욱 강력하게 해주는 **패키지** 가 없었다면 서브라임이 이렇게 인기가 있었을까? 그리고 `Package Control`은 패키지를 쉽게 설치/제거 할 수 있도록 하는 **Must have 패키지** 이다. 여기서는 간단하게 [Package Control]()을 설치하는 법을 설명하도록 한다.

### Package Control 설치
메뉴에서 `View > Show Console` 을 선택하여 콘솔창을 연다. 콘솔창이 열리면, 자신의 서브라임 버전에 맞는 코드를 복사하여 붙여넣는다.
아래 코드는 시간이 지남에 따라 변경될 수 있으니 [서브라임 공식 홈페이지](https://packagecontrol.io/installation) 에서 확인하는게 좋다.

#### Sublime Text 3의 경우
````
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
````

#### Sublime Text 2의 경우
````
import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
````

패키지 컨트롤 설치 참 쉽다. 더 자세한 사항이 궁금하다면 [Package Control 공식홈페이지](https://sublime.wbond.net/)에서 확인하시길.

### Http Requester
#### 사용방법
URL을 선택하고 `command + option + R`을 누르면 해당 리퀘스트 정보를 보여준다.

### PlainTasks
<span class="label label-success">Github</span> https://github.com/aziz/PlainTasks  
![새로운 도큐멘트 열기](http://take.ms/G5Jp5)

#### 사용방법
- `shift-command-p` -> tasks: New document
- 뒤에 `:` 부호를 붙여 테스크를 분리하는 헤더를 생성
- 할일을 적은 후에 `command+enter` 혹은 `command+i`를 누르면 앞쪽에 체크박스가 생성
- 할일목록 뒤쪽에 `@`을 입력 후 태그
- `-- + tab`으로 구분선을 생성
- 완료된 할일 위에 커서를 위치하고 `command+D`를 누르면 해당 할일이 완료
- `shift+command+A`를 눌러 완료된 할일들을 따로 보관
- `contorl+C`를 눌러 할일을 취소
- 'control+command+위 아래 방향키'로 목록을 위 아래로 이동  
    (이 기능은 서브라임 자체 기능이다.)

#### 문제점
- 한글 지원 미흡
    + 가끔씩 깨지는 경우 발생
    + 테스크를 취소하는 `contorl+C`가 한글입력상태에서 동작하지 않음
