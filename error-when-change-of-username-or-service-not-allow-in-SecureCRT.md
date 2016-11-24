---
title: SecureCRT, SSH 접속 에러 해결 Change of username or service not allow
date: 2014-12-24 17:10:52
categories:
- tools
tags:
- SecureCRT
---

SecureCRT를 사용하여 ssh 접속을 할 때 Change of username or service not allow 라는 메세지가 뜨며 접속이 되지 않을 경우 이를 해결하는 방법을 설명한다.

<!-- more -->

처음에는 서버 문제인줄 알았는데 알고보니 **SecureCRT** 의 인증창에서 username 필드를 변경했기 때문에 발생하는 에러다.

간단히 Session Option 에서 로그인하는 아이디를 변경하여 접속하면 해결된다. 그런데 계정을 변경할 때마다 설장창을 띄워 아이디를 변경하는 건 상당히 귀찮겠지?

내가 이 문제를 해결하기 위해 참고한 [원문](https://forums.vandyke.com/showthread.php?t=5490)은 왜 이런 문제가 발생했는지를 알려줄 뿐만 아니라 그 대안까지 제시하고 있다.

영어에는 약하지만 번역해보자면,

> SSH서버에 연결하는 경우 프로토콜이 인증을 요청할 때 프로토콜은 사용자 이름을 보낸다. 즉 **Enter Secure Shell Password** 다이얼로그가 보여질 때 이미 SecureCRT 세션에서 저장된 사용자 이름을 보낸 상태다.

>이는 `File -> Trace Options` 을 활성화하여 확인 할 수 있다. Trace를 암호 변경 대화 상자가 표시되기 전에 `userauth` 요청이 전송 된 것을 확인 할 수 있다.

>일반적으로 클라이언트는(SecureCRT) 사용할 수 있는 인증 방법 목록을 얻기 위해 *none* 을 보내지 않는다.

>따라서 이 시나리오에 대한 가장 좋은 해결 방법은 기존의 세션을 복제하고 세션 이름 끝에 사용할 사용자 이름을 추가한 다음 설정창에 username 필드에 각기 다른 사용자 이름을 저장하여 사용하는 것이다.

> 컨트롤 키를 누른 채로 클릭하고 오른쪽 키를 눌러 블라블라...

뒷부분은... *설정을 변경할 때는 생성한 세션들을 모두 선택해서 username을 제외한 나머지 설정사항들을 한번에 변경하면 된다...* 는 말인것 같은데... 하나를 확실히 설정해놓고 복제해서 계정명만 바꿔주면 되는것이니 크게 신경쓰지 않아도 될 듯하다.

---

#### 참고
- https://forums.vandyke.com/showthread.php?t=5490
