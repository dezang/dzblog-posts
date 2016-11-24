---
title: 효율적인 AWS 리전별 속도 측정
date: 2016-10-10 11:30:43
categories:
- aws
tags:
- latency
---

아마존은 이 포스팅을 작성하는 시점 기준으로 총 12개의 리전을 제공한다. 아무리 아키텍처의 최적화를 통해 서비스의 퍼포먼스를 개선한다고 해도 서버와 고객간의 물리적인 속도를 개선할 수 있는 방법은 오로지 고객과 서버와의 거리를 좁히는 방법 뿐이다. 따라서 AWS 인프라를 구성할 때 서비스의 고객의 국가를 고려하여 리전을 선택하고 해당 리전에 배포해야 한다.

<!-- more -->

아마존 리전에 있는 인스턴스와 내 컴퓨터와의 응답 속도를 측정하기 위해서는 간단히 ping 명령어를 사용하면 된다. 이를 위해서는 체크하고자 하는 리전에 ICMP 프로토콜에 접근을 허용한 인스턴스가 있어야 한다. 응답 속도 체크하자고 12개 리전에 인스턴스를 일일이 생성할 수는 없는 일. 아마존에서 공개한 서버가 있는 것 같긴한데 이를 위해 구글링 하는 것은 이 포스트의 목적에 벗어나기 때문에 넘어간다.

다행히도 이런 번거로움을 해결해주는 사이트가 있다. 먼저 소개할 사이트는 [cloudping](http://www.cloudping.info/) 이다.

![cloudping](https://lh3.googleusercontent.com/n1EuQ7IZhHyUIEZP8x-JOig0-K-8flU3jCg80f15kkXdsLifHXDwzEQqLnYBrdhoCNHgEu0GI6wsXENyAJafeDdj4QWcyrSDG0SJWP_UEWZmsqH295Ls7ugoAL67mKZq3GUM3io_WCvfhCAhEyaSU7iA0DSPD54KH1yk3n9GtCnkozGlQou2CIA1pxrfZ0ZEcmqJlqm3G72zbg0QE0C4Hb31iHbM20mpdyj64IWf1op3KDi97nv72XD9g2PUVIZMEsByMKZ4OBXBv9ye-wrtTIwPsKkujgli6RIQFGVOW2yone2UiFFUTFPzt4i41iE7pM6cAzBQROsobI_muPGUDuAHxxt4HAZCWZirJfUfQ7QTcJK1aWLUNnSZappQMt-dH90kkJ2HpRQXvLNoSW14Vw8yENSy84oC6gsOXo3CczRH-giEBCSQrmNCfYYvV0FeoomYRqZCdbqVOoMVug2i2NHVVw39rjXMJSCdlVcjnQWebPjvIWCLRUY0wG5RsJxnqotF-29Xh1uEjHzkJQ3XRBPPs8sTGIvNkRIEegDQL_FKviihgPSbYpj71oPrYy8pyGJzMLNfcK8b-0GJz7wMF3CaDCroUIYosps2LE2b_eWpfG_WOQ=w760-h577-no)

하단에 위치한 HTTP Ping 버튼을 누르면 모든 리전에 한번씩 Ping을 보내 응답 속도를 체크해준다. 대장은 한국에 살고 있기 때문에 서울 리전에 속도가 가장 빠르다. 반응 속도는 항상 일정한 것이 아니기 때문에 속도는 매번 랜덤하지만 대충 엇비슷하게 나온다. 더 정확도를 높히고 싶다면 여러 번의 핑을 날려보고 평균값을 확인하는 것이 좋다.

클라우드핑의 문제점은 내가 위치한 곳을 기준으로 한 응답 속도만 체크할 수 있다는 점이다. 만약 프랑스에서 서비스하는 제품이라면 한국에서 반응 속도가 빠른 리전은 그다지 필요치 않다. 이를 해결하기 위해서는 Proxy 서비스를 이용해서 해당 국가로 프록시하여 체크하는 방법이 있는데 번거롭다. 이런 불편함을 위해서 국내 기업인 [mobilfactory 에서 만든 웹사이트](http://cloudping.mobilfactory.co.kr/)가 있다.

![mobilfactory's cloudping](https://lh3.googleusercontent.com/-CErLHEiB01pacpcN6LLbfVrqYEiZZ-nmA-IKg0X9aJi6688nxzylr7zP9CMeYrV0e8a_kwPfk3KUZ9VILeyVd6EShUBHsYOd1DQ5gSzEpJmiy3U1aaO27vYDXoKJi8LrSr1VU5VBZMRZVxy62NRbOqkypQSaEUttYUASAJVGi223sWG1hpLQ-jjSBfvWRGei5bx7BGgLhsRpSSNZD4utZr3nWllbJB909uWB8fpP4chbeIkno_WRvNsudtlU9nRoOW5f5PO-RdZNnfgGnwptDYG2Qq31U-x2t3NP8TjrxTIul5M0XFy0bYgdpK2Is3epZlmsh_spw2E2UCTeC11jbt5YQEWuecvmPt0c2_BTPXTGwvMZlBdCGlxvl_EptEqcOoqqIYCQwMNSLZ5BwNW7qLorA_WK5UJcXh0jW6ygJcZYL8Rc2rJpc_2-tGGcseOBibh7cPsaxgyqPZE6q4wXZ12fswnC0ksrV-ceFylIXT2At_mQx2lBGcfj7NJYhu9V3VBu_9MdIot30BAZ9TKmmHWbRbQX1zryt5JM7WzJMmeM9RjSY1AzvEZJyUEIc3-_A1Vfb5-N62WsspIM8l4WTmedPwhQkEwlLKGH3yFl1KBttur_g=w625-h551-no)

디자인은 cloudping과 비슷하나 3번의 핑을 날려 평균값을 구해준다. 2개의 리전이 빠져있지만 베이징의 경우 어짜피 중국인이 아니면 사용할 수 없고 Mumbai 도 마찬가지이지 않을까 예상된다. 또 하나의 장점은 각 지역별 응답 속도 통계를 제공해준다는 점이다. `Show Statistics` 버튼을 누르면 아래와 같은 화면을 볼 수 있다.

![show Statistics](https://lh3.googleusercontent.com/0hSWXca-XbctB0VdxKI27EGphsfsvd3ZjvNQYFgJHGt0zJ5VR_fOYb3Z8CTQ9v42-BJ_LwGL8mEfxtl9kb5mCgpv7Qv_nMEVU-9gcRdSYlp3Yp_gKs2qUtfI4lGuxRjrKR3W7AdLyn0-KcY83bJaPXVnnd4DcMiHN3wv6ghkT8jQDwLNA-jE0kpFSsTfOunsMBGMbMeLoaRVeBVbgYNcUOD6ZQxtvVoThMxqzpr68AedSVD84Wx6OvIgDScCHLp31Ycti-1avL4R4xOFCncqbkopHZat43cqjen4VvQ5Ian8KwjD2nh6rRnx6sbnIcG9za_O1pYZY1e5ewfdT-c7vfqHkBnUyCtounUApWIVv64c4fxthRMFr9xJMLIWSoinK_3bmhqb8hbDv7O4HjdYTUKo_x21rOI6NT5fokUT1HLJ_Pia6Kr7vSHs2WKPLZmveqB5JfUVm6k0o3wkngpgCuV8ZvQfw5eN2ozgltA0tgnXT-M4r1eAy39mpyJcdxYt2fq8Co_RCUwBRLj3RG33mOrXJAj_CiNhAfUkvJU6DRs7s-G1buyodn32Fz-f7KKmwl8zVMugTU6SVOJnO0LV1QUK0DlRy6Kx6A38z7lHhQzRhxRcSQ=w1040-h553-no)

만약 프랑스와 같은 유럽권에 서비스하고 싶다면 통계표를 통해 Ireland나 Frankfurt 리전에서 서비스하면 비교적 빠른 응답 속도를 기대할 수 있을 것이라 예상할 수 있다.
