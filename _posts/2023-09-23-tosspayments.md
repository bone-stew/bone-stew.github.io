---
layout: post
title: "인앱 결제와 PG 결제의 차이점"
date: 2023-09-23 12:00:00
author: "조재룡"
catalog: true
tags:
  - 인앱 결제
  - PG 결제
  - TossPayments
---

## TossPayments를 이용해서 결제 구현
안녕하세요 조재룡입니다. 이번 프로젝트에서 왜 결제를 토스 페이먼츠로 구현했는지 간단히 설명하겠습니다.

### 인앱 결제 VS PG 결제
먼저 인앱 결제와 PG 결제를 간단히 알아야 합니다. 이번 프로젝트는 안드로이드 프로젝트이고 모바일 앱에서는 '인앱 결제' 또는 'PG 결제'를 제공합니다.

#### 인앱 결제란??

인앱 결제는 모바일 앱 내에서 Apple이나 Google의 자체 결제 시스템을 사용하는 방법인데 사용자는 Apple 또는 Google 계정에 결제수단을 미리 등록해놓고 지문이나 비밀번호로 쉽게 결제할 수 있습니다. 그래서 모바일 앱의 제공자는 Apple, Google에게 매출의 일부를 수수료로 지불합니다.

여기서 핵심은 **고객이 온라인에서 사용할 수 있는 서비스 및 콘텐츠는 반드시 인앱 결제를 사용해야 한다는 점입니다.**

#### PG 결제란??

온라인 콘텐츠가 아닌 식품, 의류 등 실물 제품들을 모바일 앱에서 판매할 예정이면 PG사로 결제를 연동해야합니다. PG를 사용하면 각 카드사, 은행과 따로 계약할 필요 없이 카드, 계좌이체, 가상계좌 등 다양한 일반결제 수단을 바로 연동할 수 있다는 장점이 있습니다. 여기서 주의 할점은 온라인 콘텐츠는 반드시 인앱 결제로 제공해야 합니다. 그 이유는 인앱 결제를 사용하지 않으면 스토어에서 앱이 제거될 수도 있기 때문입니다.

이외에도 ‘인앱결제 강제 방지법’ 때문에 생긴 제 3자 결제 방식이 있습니다.
각각 무슨 방식을 이용하는 가에 따라 수수료가 다르기 때문에 잘 알아보고 각자 맞는 방식을 쓰면 될 것 같습니다. 


### 결론과 간단한 결제 Flow

이번 프로젝트에서는 굿즈 상품들을 판매하는 것이기 때문에 인앱 결제보단 PG결제가 적합하다고 생각이 들었습니다. PG결제사(토스페이먼츠, KG이니시스, NH한국사이버결제 등등)중에서 토스페이먼츠가 우리나라에서 제일 많이 쓰고 있고 설명이 잘되어 있기 때문에 적합하다고 생각이 들었고 선택해서 개발을 진행했습니다.

![Alt text](image.png)

결제 Flow는 위의 그림과 같고 간단히 더 설명하면
1. 구매자가 구매를 요청합니다.
2. 카드사가 인증을 해줍니다.
3. 요청한 데이터에 따라서 결제 데이터를 생성해줍니다.
4. 그 결제 데이터에 따른 승인을 해줘야 하기 때문에 결제 승인 요청을 합니다.
5. 결제가 완료됩니다.

___
## 결제 프로젝트를 할 때 주의할 점
1. 토스페이먼츠 위젯을 테스트를 하기 위해 안드로이드 에뮬레이터로 테스트를 하면 결제 앱이 깔려 있지 않아서 진행이 되지 않습니다. (그래서 실제 안드로이드 폰으로 진행해야 합니다.)


2. 정책이 자주 바뀌므로 결제에 관련된 일을 한다면 정책을 자주 보는 습관을 길러야 할 것 같습니다.

<br>

## 마무리

부족한 글 끝까지 읽어주셔서 감사합니다.

이 포스팅은 다음 문헌을 참고하였습니다.
 - https://velog.io/@tosspayments/%EC%9D%B8%EC%95%B1-%EA%B2%B0%EC%A0%9C-vs.-PG-%EA%B2%B0%EC%A0%9C-%EB%AD%98-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%EB%8F%BC%EC%9A%94#%EB%AA%A8%EB%B0%94%EC%9D%BC-%EC%95%B1-%EA%B2%B0%EC%A0%9C-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%97%B0%EB%8F%99%ED%95%A0%EC%A7%80-%EA%B3%A0%EB%AF%BC%ED%95%98%EA%B3%A0-%EC%9E%88%EB%82%98%EC%9A%94