---
layout: post
title: "Spring Security를 사용한 이유"
date: 2023-09-10 12:00:00
author: "조재룡"
catalog: true
tags:
  - springSecurity
  - Jwt
---

## **SpringSecuriy를 이용해서 로그인 구현**
안녕하세요 조재룡입니다. 사용자가 로그인을 할 때 로그인의 인증 처리를 왜 스프링 시큐리티를 통해 구현했는지 소개해드리겠습니다.

### SpringSecurity를 쓴 이유
유저 입장에서 로그인은 당연한 기능이어서 신경을 쓰지 않지만 개발자 입장에서는 회원들의 개인정보가 유출되지 않도록 보안 처리를 해야하므로 어려운 기능이라고 생각합니다.

로그인 처리를 하는 방법은 먼저 Interceptor가 있습니다. Interceptor로 로그인을 처리하면 개발자가 인증,보안 기능을 직접 구현해야합니다. 하지만 SpringSecurity는 Spring에서 제공하는 인증관련 옵션을 사용하므로 난이도 높은 보안 관련 코딩을 직접하지 않아도 된다는 장점이 있어서 SpringSecurity를 도입하게 되었습니다.

스프링 시큐리티를 사용하게 되면 좋은 점을 간단히 설명하였고 더 궁금하시다면 밑에 문헌을 참고하시면 좋을 것 같습니다. ^_^

### JWT토큰을 쓴 이유

SpringSecurity는 기본적으로 세션 기반 인증을 사용하기 때문에 회원이 많아질수록 서버에 부하가 걸리게 되므로 JWT(JSON Web Token)을 이용해서 서버 부하를 줄여주고 사용자 인증 정보를 안전하게 저장하고 전달하고자 사용하게 되었습니다.

#### JWT 장점
서버에 인증 정보에 대한 세션과 같은 저장소가 필요 없어서 부하가 훨씬 덜 걸리고 서버가 상태성을 갖지 않고도 회원을 인증/인가하고 필요한 서비스를 제공해줄 수가 있는 것이 장점이라고 볼 수 있습니다.

#### JWT 단점
하지만 장점만 있지는 않습니다. JWT 처리 자체의 비용이 있으므로 인증 요청이 많아질수록 네트워크 부하가 심해질 수 있습니다. 또한 payload는 암호화가 되어있지 않으므로 유저의 critical한 정보를 담을 순 없고 만약 토큰을 탈취당한다면 대처하기 어렵다는 큰 보안적 이슈가 있습니다.

#### JWT 단점 해결 방법

![Alt text](image.png)

위의 그림을 보면 이런 문제점을 해결하기 위해서 Refresh Token이 있는 것을 확인할 수 있습니다. 그림을 보면 쉽게 이해할 수 있지만 한번 더 설명을 보태자면 토큰 탈취의 가능성을 고려해서 Access Token의 만료시간을 짧게하고 만료가 되면 클라이언트에서 서버쪽으로 Refresh Token을 보내서 유저가 맞는지 확인하고 다시 Access Token을 재발급 해주는 방식입니다.

<br>

## 마무리

부족한 글 끝까지 읽어주셔서 감사합니다.

이 포스팅은 다음 문헌을 참고하였습니다.
 - https://flamme1004.gitbook.io/flamme-dev/info-1/framework/spring/springsecurity/springsecurity
 - https://eungeun506.tistory.com/137#:~:text=JWT%20%EC%9E%A5%EC%A0%90,%EB%A5%BC%20%EC%A0%9C%EA%B3%B5%ED%95%B4%EC%A4%84%20%EC%88%98%EA%B0%80%20%EC%9E%88%EB%8B%A4.