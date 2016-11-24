---
title: 우분투 14.04에 LEMP 스택 설치하기
date: 2015-03-26 18:24:37
categories:
- ubuntu
tags:
- linux
- nginx
- mysql
- php
---

LEMP 소프트웨어 스택은 동적 웹 페이지와 웹 애플리케이션을 서비스할 수 있도록 만드는 소프트웨어의 그룹이다. LEMP는 리눅스 운영체제, 엔진엑스 웹서버의 기술 약어이다. 백엔드 데이터는 MySQL에 저장하고 동적 처리는 PHP에 의해 이루어진다. 이 포스트에서 우분투 14.04 서버에 LEMP stack을 설치하는 방법을 설명한다. 우분투는 이미 설치되었다고 가정하고 나머지 구성요소들을 설치하고 실행하는 방법을 설명한다.

<!-- more -->

## 전제 조건
이 지침을 따라 하기 전에, 서버에 일반적인 sudo 권한을 가진 루트 계정이 아닌 사용자 계정이 있어야 한다. 이것은 {% post_link initial-server-setup-with-ubuntu-14-04 %} 글을 통해 배울 수 있다.

당신이 생성한 계정으로 로그인할 수 있다면, 본 지침의 다음 단계를 시작할 준비가 된 것이다.

## 1 단계 - Nginx 웹 서버 설치
사이트 방문자에 웹 페이지를 표시하기 위해 우리는 현대적이며(?) 효율적인 Nginx 웹 서버를 사용할 것이다.

이 절차에서 모든 소프트웨어는 우분투의 기본 패키지 저장소에서 가져올 것이다. 이것은 설치를 완료하기 위해 `apt` 패키지 관리자를 사용한다는 것을 의미한다.

이 세션에서 `apt`를 사용하는 것은 처음이기 때문에 우리는 로컬 패키지 인덱스를 업데이트하고 시작해야 한다. 다음 명령어를 통해 nginx를 설치할 수 있다.

```sh
$ sudo apt-get update
$ sudo apt-get install nginx
```

우분투 14.04 에서 Nginx는 설치 시 실행하도록 구성되어 있다. 웹 브라우저에 서버 도메인이나 공인 IP를 입력하여 서버가 정상적으로 설치되었고 동작하는지 확인할 수 있다. 도메인이 아직 서버를 가리키지 않거나, 당신이 서버의 공인 IP 주소를 모른다면 터미널에 다음 명령어들 중 하나를 입력하여 확인할 수 있다.

```sh
$ ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'

111.111.111.111
FE80 :: 601 : 17ff : fe61 : 9801
```

{% alert info %}
명령어를 입력하기 어렵다면 `ip addr show eth0` 만으로도 충분히 ip를 식별할 수 있을 것이다. 유명한 `ifconfig` 명령어로도 알 수 있다. 공인 아이피가 `eth0` 에 설정되지 않고 다른 이더넷에 설정되었을 수도 있으니 개인적으로는 `ifconfig` 명령어를 사용하여 확인하는 것을 추천한다.
{% endalert %}

또는 `iconhazip` 사이트를 이용하여 알 수도 있다.

```sh
$ curl http://icanhazip.com

111.111.111.111
```

위 과정을 통해 알아낸 아이피 혹은 아이피와 연결된 도메인을 웹 브라우저 입력하자. 그러면 Nginx의 기본 랜딩 페이지를 볼 수 있을 것이다.

```
http://server_domain_name_or_IP
```

![](https://assets.digitalocean.com/articles/lemp_1404/nginx_default.png)

위의 페이지를 볼 경우, 성공적으로 Nginx가 설치된 것이다.

## 2 단계 - 사이트 데이터를 관리하기 위한 MySQL 설치
이제 우리는 웹서버를 가지게 되었으니, 사이트의 데이터를 관리하기 위해 데이터 관리 시스템인 MySQL 설치가 필요하다. 아래를 입력하여 쉽게 설치할 수 있다.

```sh
$ sudo apt-get install mysql-server
```

설치가 마지막에 MySQL의 시스템 내에서 사용하기 위한 루트(관리자) 암호를 입력하라는 메시지를 볼 수 있다. 이제 MySQL 데이터베이스 소프트웨어는 설치되었지만, 그 구성은 아직 정확히 완료되지 않았다.

첫 번째로, MySQL이 데이터베이스와 정보를 저장할 디렉터리 구조를 생성하기 위하여 다음을 입력해야 한다:

```sh
$ sudo mysql_install_db
```

다음으로, 안전하지 않은 기본값을 수정하라는 메시지를 표시하는 간단한 보안 스크립트를 실행하는 것이 좋다. 다음을 입력하여 스크립트를 시작하자:

```sh
$ sudo mysql_secure_installation
```

그럼 설치 중에 입력한 MySQL의 루트 암호를 입력해야 한다.

다음으로 비밀번호를 바꾸고 싶은지 물을 것이다. 당신의 MySQL 루트 암호가 만족스럽다면 "N"을 입력 후 엔터를 누르면 된다. 그 후 몇몇 테스트 사용자와 데이터베이스를 제거하라는 메시지가 표시된다. 안전하지 않은 기본 설정들을 제거하려면 단지 프롬프트에 "ENTER" 를 입력하면 된다. .

스크립트가 모두 실행되면, MySQL은 시작할 준비가 된 것이다.

## 3 단계 - 프로세싱을 위한 PHP 설치
이제 우리는 우리의 페이지를 제공하기 위해 Nginx 설치를 했고, 우리의 데이터를 저장하고 관리하기 위한 MySQL도 설치했다. 하지만 우리는 여전히 이 두 가지를 연결하고 동적 콘텐츠를 생성하기 위해 뭔가를 해야 한다. 이를 위해 우리는 PHP를 사용할 수 있다.

Nginx는 다른 웹 서버와 달리 네이티브 PHP 처리를 포함하지 않기 때문에, `PHP5-FPM` (FastCGI process manager)를 설치해야 한다. PHP 요청을 이 소프트웨어가 처리하도록 엔진엑스를 통해 전달할 것이다.

우리는 이 모듈과 데이터베이스 백엔드와 통신 할 수 있도록 하는 부가적인 헬퍼 패키지를 함께 설치할 것이다. 이 설치작업은 필수적인 PHP 코어 파일이 필요하다. 다음을 입력하여 이를 수행한다.

```sh
$ sudo apt-get install php5-fpm php5-mysql
```

### PHP 프로세서 설정
이제 PHP 구성 요소가 설치되어 있지만, 더욱 안전하게 하기 위하여 약간의 변경이 필요하다.

루트 권한으로 `php5-fpm` 설정 파일을 연다:

```sh
$ sudo vim /etc/php5/fpm/php.ini
```

이 파일에서 `cgi.fix_pathinfo` 라는 설정 매개 변수를 찾는다. 이것은 세미콜론으로 주석처리 돼 있고 기본적으로 1로 설정되어 있을 것이다. 이것은 매우 안전하지 않은 설정이다. 이유는 PHP 파일이 정확히 일치하지 않는 경우 가장 근접한 파일 실행을 시도 하는 설정이기 때문이다. 이 설정이 허용되지 않도록 설정해야 사용자가 원하는 방식으로 PHP 요청을 할 수 있다.

라인의 주석을 풀고 "0"으로 설정하여 취약점을 제거한다.

```
cgi.fix_pathinfo = 0
```

저장이 완료되면 파일을 닫는다.

이제, 다음을 입력하여 PHP 프로세서를 다시 시작한다 :

```sh
$ sudo service php5-fpm restart
```

이것은 변경 사항을 적용할 것이다.

## 4 단계 Nginx가 PHP 프로세서를 사용하도록 설정
이제 우리는 설치에 필요한 모든 구성 요소를 가지고 있다. 앞으로 해야 할 유일한 구성 변경은 동적 콘텐츠에 대한 PHP 처리기를 Nginx가 사용하도록 하는 것이다.

우리는 서버 블록에서 이 작업을 수행한다. (서버 블록은 아파치의 가상 호스트와 유사하다.) 다음을 입력하여 기본 Nginx 서버 블록 구성 파일을 연다.

```sh
$ sudo vim /etc/nginx/sites-available/default
```

{% alert info %}
이 원문의 마지막 업데이트는 2014년 7월. 이 글을 번역하는 2015년 3월 26일 기준으로 디폴트 엔진엑스 서버 블록 구성 파일의 위치는 `/etc/nginx/conf.d/default.conf` 이다.
{% endalert %}

주석처리가 된 부분을 제거하면, 엔진엑스의 기본 서버 블록 파일은 아래와 같을 것이다.

```sh
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    root /usr/share/nginx/html;
    index index.html index.htm;

    server_name localhost;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

*내 기본 설정파일이랑 좀 다른데? ㅠㅠ*

사이트를 위해 약간의 변경이 필요하다

- 디렉터리가 요청될 때 PHP index 파일이 제공될 수 있도록 index 지시자에 `index.php` 를 추가한다.
- `server_name` 지시자가 자신의 도메인 주소 또는 공인 IP를 가리키도록 수정한다.
- 현재 구성 파일은 오류 처리 루틴을 정의하는 몇 라인이 주석처리 되어있다. 이 기능을 포함하도록 주석을 해제한다.
- 실제 PHP 처리를 위해, 다른 섹션의 일부를 주석을 해제해야 한다. 또한 Nginx가 잘못된 요청을 PHP 프로세서에 전달하지 않도록 하기 위해서`try_files` 지시자를 추가해야 한다.

위 사항들을 변경하면 서버 블록은 다음과 같을 것이다.

*원문에는 변경된 부분이 빨간색으로 표시되어 있지만 내 블로그 테마의 설정상 코드 내부의 특정 라인 색깔을 변경하는 게 어려워 그냥 옮겼다.*

```sh
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    root /usr/share/nginx/html;
    index index.php index.html index.htm;

    server_name server_domain_name_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```

*이 부분은 내 블록 파일과 많이 달라서 추가한 부분과 비슷한 주석 처리된 부분의 주석을 해제하기만 하였다. 서버 블록 설정에 대해서는 다른 최신 글을 통해 더 조사해봐야 할 듯*

위의 변경한 뒤 파일을 저장하고 닫는다.

변경한 것들을 적용하기 위해 Nginx를 재시작한다.

```sh
$ sudo service nginx restart
```

## 5번째 단계 - 설정 테스트를 위한 PHP 파일 생성

이제 LEMP 스택은 완전히 설정되었다. 이제 Nginx가 `.php` PHP 프로세서로 확실히 핸들링하는지 확인해봐야 한다.

문서 루트에서 테스트 PHP 파일을 만들어 이 작업을 수행할 수 있다. 텍스트 편집기로 문서 루트 안에 `info.php` 파일을 생성하고 연다.

```sh
$ sudo vim /usr/share/nginx/html/info.php
```

서버의 정보를 반환하여 보여주는 php 코드를 입력한다.

```php
    <?php
        phpinfo ();
    ?>
```

작업이 완료되면 파일을 저장하고 닫는다. 이제 서버의 도메인 이름 또는 공인 IP 주소를 방문하여 웹 브라우저에서 이 페이지를 방문한다.

```
HTTP : // server_domain_name_or_IP /info.php
```

별다른 문제가 없다면 서버에 대한 정보를 PHP에 의해 생성된 웹 페이지를 볼 수 있을 것이다

![](https://assets.digitalocean.com/articles/lemp_1404/php_info.png)

이처럼 보이는 페이지를 볼 경우, 당신은 성공적으로 Nginx에 PHP 처리를 설정한 것이다.


이 테스트 후에는 파일을 제거하도록 하자. 제거하지 않는다면 이 파일은 허가받지 않은 사용자들이 침입을 시도하는 데 도움이 될 수 있는 정보를 제공하게 될 것이다.

아래를 입력하여 파일을 제거한다.

```sh
 sudo rm /usr/share/nginx/html/info.php
```

## 결론
이것으로 우분투 14.04 서버에 LEMP 스택 구성을 완료하였다. LEMP 스택은 당신의 방문자에게 웹 콘텐츠를 제공하기 위한 매우 유연한 기반을 제공할 것이다.

---

#### 참고
- https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04
