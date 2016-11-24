---
title: SSH 키 생성, 등록, 사용 방법
categories:
- tools
tags:
- ssh
---

SSH 키 생성하고 생성된 SSH키를 등록하고 사용하는 방법을 설명한다.

<!-- excerpt -->

## 키 확인
```sh
$ ls -al ~/.ssh
```

## 새로운 SSH 키 생성
```sh
$ ssh-keygen -t rsa -b 4096 -C "dezang@dezang.net"
```

## SSH 에이전트에 생성한 SSH 키 등록
###  SSH 에이전트 확인
```sh
$ eval "$(ssh-agent -s)"

Agent pid 27986
```

실행되고 있지 않고 있다면, 아래 명령어로 실행
```sh
$ ssh-agent /bin/bash
```

### 키 등록
```sh
$ ssh-add ~/.ssh/id_rsa

# 등록된 키 확인
$ ssh-add -l

4096 SHA256:7ocRRpWHQxRSRqsco//QuxhIE74U3dPlQfWvnDikzN8 /Users/dezang/.ssh/id_rsa (RSA)
```

## SSH 키 사용
```sh
$ pbcopy < ~/.ssh/id_rsa.pub
```

### github
키 등록 후 ...

```sh
$ ssh -T git@github.com

Hi dezang! You\'ve successfully authenticated, but GitHub does not provide shell access.
```

### bitbucket
키 등록 후 ...
```sh
$ ssh -T git@bitbucket.org                                                
logged in as Dezang.

You can use git or hg to connect to Bitbucket. Shell access is disabled.
```

### ubuntu server
```sh
$ mkdir ~/.ssh
$ vim ~/.ssh/authorized_keys
# 해당 파일에 public 키 정보를 붙여넣는다.
```

#### 참고
- https://help.github.com/articles/generating-an-ssh-key/
- https://confluence.atlassian.com/bitbucket/set-up-git-744723531.html
