---
title: 우분투 GPG 공개키 인증 오류 해결
date: 2016-07-18 14:38:47
categories:
- ubuntu
tags:
- gpg
---

`apt-get` 을 통한 업데이트 도중 GPG 오류 발생시 해결 방법을 기술한다.

<!-- excerpt -->

## 문제발생
보안을 위해 버전은 항상 최신으로! 라는 마인드로 살아가는 황대장. 오늘도 어김없이 서버에 접속해서 업데이트를 위해 다음을 입력한다.

```sh
$ sudo apt-get update
```

퍼센트를 마구 올려대며 무언가를 막 하는 것 같더니 내뱉는 에러한 줄

```sh
W: GPG 오류: http://nginx.org trusty Release: 다음 서명들은 공개키가 없기 때문에 인증할 수 없습니다: NO_PUBKEY ABF5BD827BD9BF62
```

자 해결해보자!

## 해결방법

```sh
$ gpg --keyserver keyserver.ubuntu.com --recv ABF5BD827BD9BF62

gpg: directory '/home/dezang/.gnupg' created
gpg: new configuration file '/home/dezang/.gnupg/gpg.conf' created
gpg: WARNING: options in '/home/dezang/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring '/home/dezang/.gnupg/secring.gpg' created
gpg: keyring '/home/dezang/.gnupg/pubring.gpg' created
gpg: requesting key 7BD9BF62 from hkp server keyserver.ubuntu.com
gpg: /home/dezang/.gnupg/trustdb.gpg: trustdb created
gpg: key 7BD9BF62: public key "nginx signing key <signing-key@nginx.com>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```

```sh
$ gpg --export --armor ABF5BD827BD9BF62 | sudo apt-key add -

OK
```

다시 업데이트 실행하면? 끝.

```sh
$ sudo apt-get update
```

## 깊이 더 깊이
### GPG 란?

> GPG는 GNU Privacy Guard (GnuPG)의 줄임말로서 배포 파일의 인증을 확인하는데 사용되는 자유 소프트웨어 패키지를 말합니다. 예를 들면 Red Hat은 비밀키 (개인키)를 사용하여 패키지를 잠근 후 공개키를 사용하여 패키지를 잠금 해제 후 검증합니다. 만일 RPM 검증 과정에서 Red Hat에서 배포한 공개키가 비밀키와 일치하지 않는다면, 패키지가 변경되었을 수 있으므로 신뢰할 수 없습니다.
>via redhat

---

#### 참고
- http://dreamlog.tistory.com/76
- http://blog.simplism.kr/?p=338
- http://en.wikipedia.org/wiki/GNU_Privacy_Guard
