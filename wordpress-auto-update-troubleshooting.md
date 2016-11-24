---
title: 워드프레스 자동 업데이트 실패 해결 방법
date: 2015-03-12 10:03:59
categories:
- blog
tags:
- wordpress
---

얼마 전 워드프레스가 4.0에서 4.1로 업데이트가 이루어졌다. 가상호스팅서버 에서는 자동 업데이트가 문제 없이 이루어졌었는데, Google Compute Engine으로 블로그를 이전하고 나서 아래와 같은 오류를 내뿜으며 업데이트가 이루어지지 않는다. 아... 또 어디가 잘못된 걸까?

<!-- more -->

```
업데이트 압축 푸는 중.
몇 개의 파일을 복사할 수 없기 때문에 업데이트는 설치할 수 없습니다. 이것은 보통 파일의 퍼미션이 서로 다르기 때문입니다.: wp-admin/includes/update-core.php
설치 실패
```

이상한 건 플러그인과 테마의 설치와 업데이트는 문제없이 수행한다는 것인데... 워드프레스 업데이트 과정에서 변경이 요구되는 파일의 퍼미션이 뭘까?

마음같아서는 워드프레스 폴더에 777 권한을 줘버려서 퍼미션 문제로부터 영원히 해방되고 싶지만, 이런 작은 귀차니즘 때문에 보안에 취약하게 만들 수는 없다. 정확한 원인을 분석하고 해결해보자!

.  
.  
.  
3시간 구글링! 우오오오오!!! \../
.  
.  
.  

하아... 아무리 찾아봐도 뭐가 문제인지 모르겠다. 앞서 해결해보자. 라고 적고 이래저래 찾아본게 3시간정도 된 것 같다.화가 난다!!

워드프레스가 설치된 폴더에서 모든 파일에 분노의 777 권한을 부여한다. 보안상 아주 취약해지는 행위지만 일단 이 업데이트 문제부터 해결하고 싶다. 모든 폴더에 777 권한을 부여하니 업데이트가 문제없이 이루어진다. ~~진작에 이렇게 할껄!!...~~ 거기다가 전부터 실패해왔던 번역 업데이트도 이루어졌다. 퍼미션에 문제가 있긴 있는듯.

업데이트를 해결했으니 다시 폴더는 775권한, 파일은 664권한을 부여한다. 워드프레스에서 [권장하는 설정값](http://codex.wordpress.org/Changing_File_Permissions)이다. `find` 명령어와 `chmod` 명령어를 적절히 혼합하여 한방에 해결하자.

```sh
sudo find . -type f -exec chmod 644 {} +
sudo find . -type d -exec chmod 755 {} +
```

다시 설정값이 돌아왔다. 도대체 어떤 파일 or 폴더의 퍼미션을 어떻게 변경해야 하는지 궁금하다. 워드프레스 문서를 찬찬히 읽어보거나 로또의 확률로 이 글에 댓글을 달아서 알려주실 해결사를 만나길 기대해봐야겠다.

뭐 원인은 분석하고 해결하진 않았지만 어찌되었든.. 해결^^;; 다음 버전 업그레이드 전까지 문제점을 알아야 할 터인데 걱정이다.


#### 참고
- http://www.smashingmagazine.com/2014/05/08/proper-wordpress-filesystem-permissions-ownerships/
- http://codex.wordpress.org/Changing_File_Permissions