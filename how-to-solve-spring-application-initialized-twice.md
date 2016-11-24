---
title: 스프링 초기화 중복 문제 해결
categories:
- develop
tags:
- spring
---

스프링을 사용하여 웹 서비스를 개발하면서 이것 저것 건드리다보니 언제부터인지 톰캣 서버를 가동하면 빈 초기화가 두 번씩 일어나기 시작했다. 빈 스캐너가 두 번 동작하면서 같은 빈을 두 번씩 생성하니 서버 재시작도 두 배 느리고, 빈이 중복으로 생성될 가능성도 배제 할 수 없었다. ~~(가비지 컬렉터가 중복된 빈을 자동으로 제거할 것 같긴한데)~~

<!-- more -->

무시하고 계속 개발했지만 서비스 오픈 일자가 코 앞으로 다가와서 더 이상 이 문제를 무시하고 진행할 수 없었다. 내가 불편하기도 했고...

방법은 너무나도 간단했다. ~~I love Stack Over Flow~~

{% quote Houcem Berrayana http://stackoverflow.com/questions/20704086/spring-application-initialized-twice  Stack Overflow %}
That's very normal. You have an application listener that loads a the context, and you have a servlet that loads on startup and loads the context also. Remove load on startup from the servlet declaration in web.xml.
{% endquote %}

web.xml 에서 servlet에 선언되어진 load-on-startup 부분을 지워주면 되는 것.구글링과 스프링 서적에 설정 환경을 확실히 이해하지 않은 상태로 선언해서 생긴 문제였다.

### 변경 전

    <servlet>
      <servlet-name>spring</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/config/spring-servlet.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
    </servlet>

### 변경 후

    <servlet>
      <servlet-name>spring</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/config/spring-servlet.xml</param-value>
      </init-param>
    </servlet>
