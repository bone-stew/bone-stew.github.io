---
layout: post
title: "RestDocs와 OpenAPI Specification을 이용한 효과적인 API 문서화"
date: 2023-09-24 12:00:00
author: "서명현"
catalog: true
tags:

- backend
- API Docs

---

안녕하세요, 서명현입니다. 🤚

저희 팀은 API 문서를 작성할 때 RestDocs와 OpenAPI Specification을 사용하고 있는데요.<br>
많은 기업에서 이미 Swagger를 사용하고 있지만, 저희 팀이 다른 방식을 선택한 이유에 대해 궁금해하실 것 같습니다.
이번 포스팅에서는 저희 팀이 RestDocs와 OpenAPI Specification을 선택한 이유와 어떤 장단점이 있는지에 대해 소개드리겠습니다.

## 목차 소개

이번 포스팅은 다음과 같은 순서로 진행됩니다.

1. [RestDocs와 Swagger의 장단점](#1-restdocs와-swagger의-장단점)
2. [우리 팀이 RestDocs를 선택한 이유](#2-우리-팀이-restdocs를-선택한-이유)
3. [OpenAPI Specification이란?](#3-openapi-specification이란?)
4. [OpenAPI Specification을 적용하게 된 이유](#4-openapi-specification을-사용하는-이유)

## 1. RestDocs와 Swagger의 장단점

### Swagger

Swagger는 API 문서를 작성할 때 가장 많이 사용되는 도구인데요.<br>
**API 문서에서 직접 테스트**할 수 있고, 어노테이션을 이용해서 **문서를 쉽게 작성할 수 있다**는 장점이 있습니다.<br>

이러한 편리함 때문인지 많은 기업에서는 Swagger를 자연스럽게 선택할 수 밖에 없는 상황이 되곤 하는데요.
더불어서 그동안의 많은 프로젝트를 진행하면서 만나뵀던 주변의 프론트엔드 개발자분들 또한 Swagger를 강력하게 선호한다는 것도 체감할 수 있었습니다.
직접 테스트할 수 있다는 장점 때문에 말이죠
<br>

하지만 Swagger를 사용하면서 느꼈던 단점도 있습니다.<br>
프로덕션 코드에 문서화를 위한 코드가 들어가기 때문에 API 스펙을 위한 코드를 분리해서 관리하기 어렵고,
검증되지 않은 API가 생성될 수 있다는 점이었습니다.

<!-- swagger 사진 -->

### RestDocs

RestDocs는 Spring에서 제공하는 도구로, **테스트 코드를 기반**으로 API 문서를 작성하는 방식입니다.<br>
테스트 코드를 작성하면서 문서화를 동시에 진행할 수 있기 때문에 API 스펙을 위한 코드와 프로덕션 코드를 분리해서 관리할 수 있습니다.
동시에 API 스펙을 검증할 수 있기 때문에 검증되지 않은 API가 생성될 가능성이 적습니다.<br>

이렇게 RestDocs는 Swagger의 단점을 보완할 수 있는데요.<br>
그대신 직접 테스트를 할 수는 없으며, 테스트 코드를 기반으로 하기 때문에 어느 정도의 시간이 더 걸린다는 단점이 있습니다.
<br>

Swagger의 단점을 보완할 수 있는 RestDocs도 최근 여러 기업에서 사용하고 있는 것으로 알고 있습니다.
_(테스트 코드를 필수로 하지 않는 팀이라면 잘 선택하지 않는 방법이기도 합니다)_

<!-- restdocs 사진 -->

## 2. 우리 팀이 RestDocs를 선택한 이유

저희 팀은 테스트 코드 작성을 기반으로 하는 RestDocs가 신뢰성 측면에서 더 좋았지만, 직접 테스트를 할 수 없다는 점이 아쉬웠습니다.<br>

고민하던 중에 RestDocs를 사용하면서 Swagger UI를 함께 사용할 수 있는 OpenAPI Specification를 알게 되었고,
RestDocs와 Swagger의 장점을 모두 취할 수 있는 방법이라고 생각되어 적용하게 되었습니다.

## 3. OpenAPI Specification이란?

OpenAPI Specification은 Swagger의 스펙을 기반으로 만들어진 API 스펙 문서입니다.<br>
Swagger UI를 사용하면서 문서를 작성할 수 있기 때문에 Swagger의 장점을 모두 취할 수 있습니다.<br>


## 마무리

저희 팀은 RestDocs와 Swagger의 장단점을 고려해서 OpenAPI Specification을 적용하게 되었는데요.<br>
적용 방법은 시간상 자세히 다루지 못하겠지만, 저희 팀에서 적용한 결과물을 보시면 이해가 더 쉬우실 것 같습니다.

- [RestDocs API 문서](https://popmate.xyz/docs/index.html)
- [OpenAPI Specification을 이용한 Swagger-UI](https://popmate.xyz/openapi/index.html)

팀에서 기존에 사용하던 방식이 있다면 따라가겠지만,
선택할 수 있는 기회가 주어진다면 각각의 장단점을 고려해서 팀의 상황에 맞게 선택하시면 좋을 것 같습니다.

이번 포스팅에서는 저희 팀이 RestDocs와 OpenAPI Specification을 선택한 이유와 어떤 장단점이 있는지에 대해 소개드렸습니다.<br>
이상으로 포스팅을 마치겠습니다. 🖐️<br>
끝까지 읽어주셔서 감사합니다.
