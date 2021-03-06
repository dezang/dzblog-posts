---
title: 서브 도메인 및 가상 호스트 설정하기
date: 2014-08-29
categories:
- server
tags:
- domain
---

워드프레스 블로그를 운영하면서 진행했던 서브 도메인 설정 방법과 가상 호스트 설정에 대해 설명한다.

<!-- excerpt -->

{% alert warning %}
이 글은 과거, 서버에 워드프레스 블로그를 직접 운영할 당시 작성한 글이다. 현재는 [Hexo](https://hexo.io/) 를 사용하여 github page 위에서 돌아가고 있다.
{% endalert %}

---

현재 글을 쓰는 시점에서 블로그에 접속하는 주소는 dezang.net/blog이다. 이를 blog.dezang.net 주소로 접속하게 하려면 서브 도메인 설정을 해야한다. 원하는 서브 도메인을 레코드 타입 A로 설정하고 서브 도메인에 연결할 IP나 레코드 값을 입력하면 된다.

문제는 IP를 입력하기 때문에 웹 서버의 루트와 연결되는 것이지 루트 아래 폴더에 서브 도메인을 연결할 수는 없다는 것. 즉 다른 아이피를 가진 서버에 서브 도메인 설정은 문제없지만 동일한 서버의 루트 아래 폴더에 서브 도메인을 설정하는 것은 네임 서버 설정만으로는 불가능하다.

직접 네임 서버를 설정해서 운영해볼까도 생각해보았지만, 그러기에는 위험 부담이 크다. 네임 서버가 죽으면 dezang.net이 포함된 어떤 주소로도 접속이 되지 않기 때문이다. 도메인 임대 서비스를 제공하는 곳은 대부분 무료 네임서버를 제공하므로 안전성을 위해서라도 무료 네임서버를 사용하는 것이 좋다. 만약 슬레이브(보조) 서버까지 운영할 여유가 있다면 직접 구성하는 것도 나쁘지는 않겠지만.

그럼 어떻게 해야 할까? 방법은 가상 호스트를 사용하는 것이다. 모든 서브 도메인을 일단 같은 서버주소로 설정하고 서버에서 가상 호스트를 통해 루트 아래 원하는 폴더로 연결한다. 그러면 dezang.net/blog로 접속해야만 했던 주소가 blog.dezang.net으로 바뀌게 되는 것이다! 지금부터 한 단계씩 진행해보자.

## 네임서버 설정하기

서브 도메인에 원하는 서브 도메인명을 넣고, IP 주소는 현재 서버의 IP 주소를 넣고 적용한다. 네임 서버를 직접 구성하지 않았다면 업체가 일정시간이 지난 후 반영해 줄 것이다. 대장은 호스팅케이알에서 도메인을 구입하고 그곳에서 제공하는 네임 서버를 사용하고 있다.

![호스팅케이알 네임서버 설정](http://take.ms/0J7mT)

## 서브 도메인 설정 관리
### 가상호스트 설정파일 작성
위 설정을 마치면 모든 서브 도메인은 아파치에서 설정한 웹 루트 디렉토리와 연결된다. 별다른 설정을 하지 않았다면 우분투를 기준으로 `/var/www/html` 가 그 곳이 될 것이다. 블로그가 설치된 폴더의 경로는 `/var/www/html/blog`가 될 것이고, 이제 서버에서 가상 호스트를 통해 blog.dezang.net으로 들어오는 요청을 `/var/www/html/blog`에 연결하면 된다.

설정 파일은 `/etc/apache2/site-enable` 디렉토리 안에 존재한다. *(ubuntu 14.01.1 LTS 기준)* 해당 디렉토리로 들어가면 `000-default.conf` 설정파일이 존재한다.

```sh
$ vim /etc/apache2/site-enable/000-default.conf
```

파일에 제일 아래 쪽에 다음을 추가한다.

```
<VirtualHost *:80>
  ServerName blog.dezang.net
  DocumentRoot /var/www/html/blog
</VirtualHost>
```

아파치 설정이 변경되었으므로 아파치 서비스를 재시작해준다.

```sh
$ service apache2 restart
```

이제 `blog.dezang.net`을 입력하면 설정 파일에 명시한 경로로 접근하게 된다. 대장은 모든 설정을 완료하였으나 결국 blog.dezang.net의 설정을 포기하고 루트도메인에 연결되도록 변경하였다.

## Rewrite Engine 설정의 복잡함
본 블로그는 워드프레스의 고유 주소 기능을 사용하는데 이는 .htaccess 파일에 rewrite 설정을 통해 이루어진다. 자세한 이유는 모르지만 서브 도메인으로 설정하면 이 .htaccess도 이에 맞게 변경해야 고유주소(퍼멀링크)가 문제없이 동작하는데 그 부분이 정규식을 잘 모른다면 수정하기가 번거롭다.

서브 도메인을 적용하자 블로그의 초기페이지는 문제없이 접근되는데 포스트 페이지나 관리자 페이지에 접근하는데 문제가 생겼다. 아예 안되진 않고 됐다가 안됐다 그러니까 귀찮더라. 추후에 .htaccess 파일 설정을 충분히 숙지한 다음에 변경해 봐야겠다.

워드프레스 고유주소를 사용하지 않거나 다른 서비스와 연결할 용도로 사용한다면 위에 설정만으로도 충분할 듯 싶다. 서브 도메인으로 자신의 웹 서비스들의 접근 주소를 보기 좋게 변경해보자.

---

#### 참고
- http://tuwlab.com/ece/5863
