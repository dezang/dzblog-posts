---
title: adduser vs useradd
date: 2016-07-28 10:56:57
categories: ubuntu
tags: command
---

우분투에서 유저를 만드는 명령어 두 가지 `adduser` 와 `useradd`. 이 단어의 순서만 다른 두 명령어의 차이점은 무엇인지 알아보고 앞으로 어떤 명령어로 유저를 생성할지 딱 정해보자.

<!-- more -->

## useradd

> useradd is a **low level utility** for adding users. On Debian, administrators should usually use adduser(8) instead.

> When invoked without the -D option, the useradd command creates a new user account using the values specified on the command line plus the default values from the system. Depending on command line options, the useradd command will update system files and may also create the new user's home directory and copy initial files.

> By default, a group will also be created for the new user (see -g, -N, -U, and USERGROUPS_ENAB).

여기서 주의깊게 봐야할 부분은 low level utility 라는 점. 계정을 생성할 때 모든 부분을 명시해줘야 한다. 즉 UID, GID 부여, 홈디렉토리 설정 등 일일이 모든 작업을 진행해줘야 한다. 반면에...

## adduser

> adduser and addgroup add users and groups to the system according to command line options and configuration information in /etc/adduser.conf. They are friendlier front ends to the low level tools like useradd, groupadd and usermod programs, by default choosing Debian policy conformant UID and GID values, creating a home directory with skeletal configuration, running a custom script, and other features. adduser and addgroup can be run in one of five modes:

`useradd, groupadd and usermod programs` 과 같은 low level 도구들을 사용하여 유저를 생성해 준다. 관리자의 입장에서는 `adduser` 명령어가 휠씬 편리할 듯.


## 결론
따라서 대장은 앞으로 `adduser`를 쓰는 걸로 결정. adduser는 적지 않음 옵션을 제공하는데 자세한 건 `man` 명령어를 통해 직접 알아보도록 하고, 대장이 필요한 옵션은 두 가지.

- ssh키를 사용하여 접속하기 때문에 비밀번호는 필요치 않다.
- shell은 bash가 아닌 zsh을 사용한다.

위 두 가지를 만족하며 사용자를 생성하는 명령어는 아래와 같다.

```
$ sudo adduser --disabled-password --shell /bin/zsh [USER_NAME]
```

#### 참고
- http://kit2013.tistory.com/187
