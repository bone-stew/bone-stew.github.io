---
layout: post
title: "stomp redis mongodb 활용 채팅 서비스"
subtitle: "popmate 채팅 시스템"
date: 2023-08-27 12:00:00
author: "김우원"
catalog: true
tags:
  - backend
  - stomp
  - redis
  - mongodb
---

안녕하세요 김우원입니다 :hand:

이번 포스트에서는 popmate의 초기 오픈 채팅 시스템이 어떻게 구현되어있는지 소개해드리겠습니다!

# STOMP

> Simple/Streaming Text Oriented Messaging Protocol

우선 STOMP에 대해 알아보기 전에 WebSocket에 대해 간략히 알아봅시다.

- WebSocket
  - 기존의 단방향 HTTP 프로토콜과 호환되어 양방향 통신을 제공하기 위해 개발된 프로토콜
  - 클라이언트가 접속 요청을 하고 웹 서버가 응답 후 연결을 끊지 않고 유지하여 클라이언트의 요청없이 데이터를 전송할 수 있는 프로토콜
  - 최초 접속이 HTTP Request를 통해 HandShaking 과정이 이뤄져 추가적인 방화벽 설정이 필요 없다.

STOMP는 메세정 전송을 효율적으로 하기 위해 탄생한 프로토콜로써, WebSocket 위에서 동작하며, 메시지 브로커라는 것을 활용하여, 클라이언트와 서버가 쉽게 메시지를 주고 받을 수 있도록 하는 프로토콜 입니다.

STOMP를 사용한 이유는

1. WebSocket만 사용해서 구현하게 되면, 해당 메시지가 어떤 요청인지, 어떤 포맷인지, 통신 과정을 어떻게 처리할지가 정해져 있지 않아 일일이 구현해야한다. 
2. STOMP 프로토콜은 송신자와 수신자를 지정하고, 메시지 브로커를 통해 특정 사용자에게만 메시지를 전송하는 기능을 간편하게 구현할 수 있다.

