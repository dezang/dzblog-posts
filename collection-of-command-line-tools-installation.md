---
title: 커맨드 라인 도구 설치 명령어 모음
date: 2016-10-10 15:34:21
categories:
- tools
tags:
---

대부분의 앱/도구들이 GUI로 나오고 있지만 개발을 하다보면 CLI에서 필요한 도구들이 존재한다. 각 도구들의 대한 자세한 설명이나 사용법은 다른 글에서 소개하기로 하고 여기서는 설치하는 명령어와 참조 링크만 작성하도록 한다. 대장의 글은 언제나 맥 기준이다.^^

<!-- more -->

## Top priority
### homebrew
```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
> Homebrew는 Apple 에서 제공하지 않는 유용한 패키지 관리자를 설치합니다.

homebrew 는 맥에 존재하는 축복이다. 대부분의 커맨트 라인 툴들은 brew를 통한 설치를 지원하고 있는 듯하다. 맥을 사용한다면 터미널을 열어 가장 먼저 쳐야할 명령어라고 생각한다.

- http://brew.sh/index_ko.html

### oh-my-zsh
```bash
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

- [oh-my-zsh](oh-my-zsh.md)
- https://github.com/robbyrussell/oh-my-zsh

### java
```
$ brew update
$ brew tap caskroom/cask
$ brew install brew-cask
$ brew cask install java
```
- http://davidcai.github.io/blog/posts/install-multiple-jdk-on-mac/
- http://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac
- http://www.jenv.be

### node
```sh
$ brew install node
$ npm install npm -g // Updating npm
```
- https://nodejs.org/en/download/package-manager/

### gradle
```bash
$ brew install gradle
```

## For front end develop
프론트 앤드 개발을 위해서 필요한 툴들이다. 이 녀석들 덕분에 프론트 개발 할 맛이난다.

### gulp
```sh
$ npm install -g gulp-cli
```

> Gulp 는 Nodejs 의 스트림(Stream) 을 기반으로 하는 빌드시스템입니다

- http://gulpjs.com
- https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md
- http://programmingsummaries.tistory.com/356

### grunt (optional)
```bash
$ npm install -g grunt-cli
```

### bower
```bash
$ npm install -g bower
```
- http://bower.io

### yoman
```bash
$ npm install -g yo
```
- http://yeoman.io

## ETC
그 밖에 필요한 도구들.

### pip
```bash
$ sudo easy_install pip
```

### aws-cli
```bash
$ sudo pip install awscli
```
