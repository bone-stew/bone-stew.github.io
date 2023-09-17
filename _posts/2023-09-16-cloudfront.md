---
layout: post
title: "React 배포"
subtitle: "S3, cloudfront"
date: 2023-09-16 12:00:00
author: "김우원"
catalog: true
tags:
  - DevOps
  - CI/CD
  - S3
  - CloudFront
---

안녕하세요 김우원입니다. 
오늘 드디어 Popmate BackOffice를 배포했습니다.
이번 글에서는 Popmate BackOffice의 CI/CD 환경을 소개해 드리겠습니다.

## 아키텍처

<img src="/img/image-20230917213421768.png" alt="image-20230917213421768" style="zoom: 50%;" />



1. 개발자가 github에 push를 하면 github action을 통해 S3 빌드 파일을 올린다.
2. CloudFront를 통해 S3의 빌드 파일을 캐싱하고, SSL을 적용한다.
3. Route 53을 통해 도메인과 CloudFront를 연결한다.

## Amazon S3

> Amazon Simple Storage Service
> 확장성, 데이터 가용성, 보안 및 성능을 제공하는 객체 스토리지 서비스

React로 웹사이트를 개발하고, 빌드 하면, HTML, JS, CSS 등 정적 파일이 생성됩니다.
해당 파일들을 웹 서버를 통해 사용자에게 제공하면, 사용자는 우리가 만든 화면을 볼 수 있게 됩니다.
S3의 웹 호스팅 기능을 사용하면, 별도의 웹 서버 설정 없이 사용자에게 전송할 수 있어 많이 사용합니다.
단, S3를 통해 웹 페이지를 호스팅 하면 2 가지 문제가 발생합니다.

1. SSL 인증서 적용이 불가능하다.
2. 도메인 적용이 안된다.

## CloudFront

> Amazon CloudFront는 .html, .css, .js 및 이미지 파일과 같은 정적 및 동적 웹 콘텐츠를 사용자에게 더 빨리 배포하도록 지원하는 웹 서비스입니다. CloudFront를 통해 서비스하는 콘텐츠를 사용자가 요청하면 지연 시간이 가장 낮은 엣지 로케이션으로 요청이 라우팅되므로 가능한 최고의 성능으로 콘텐츠가 제공됩니다. - 출처 Amazon

CloudFront를 사용하면 크게 두 가지 이점을 가져갈 수 있습니다.

1. 빠르다.

   엣지 로케이션이라고 하는 전 세계 네트워크를 통해 오리진(여기선 S3)으로부터 캐싱한 콘텐츠를 제공하기 때문에 최고의 성능으로 콘텐츠가 제공됩니다.

2. SSL 인증서를 통한 https 호스팅이 매우 쉽다.

   ACM을 통해 인증서를 받고 CloudFront에 적용할 수 있습니다.

CloudFront는 오리진에서 일정 시간마다 콘텐츠를 가져와 캐싱 하는데, S3에 배포 후, 바로 적용 시키기 위해서는 초기화 작업이 필요합니다.

## Route 53

> 가용성과 확장성이 뛰어난 DNS(도메인 이름 시스템) 웹 서비스

popmate의 도메인 popmate.xyz의 서브 도메인 dashboard.popmate.xyz를 CloudFront와 연결하기 위해 사용합니다. 

## Github Action

> **GitHub** Actions는 **GitHub**에서 제공하는 서비스로, 빌드, 테스트, 배포 파이프라인을 자동화할 수 있는 CI(Continuous Integration, 지속 통합)와 CD(Continuous Deployment, 지속 배포) 플랫폼입니다.

깃헙 액션을 통해 다음과 같은 작업을 수행합니다.

1. main에 push가 되면 실행됩니다.
2. aws IAM 계정을 설정합니다.
   - s3, cloudfront에 대한 권한이 있어야 합니다
3. node module 설치 후, build 합니다.
4. S3에 빌드 파일을 업로드 하고, CloudFront의 캐시를 무효화합니다.

```bash
name: CI/CD popmate_bo to AWS S3

on:
  push:
    branches:
      - main
      - develop

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v4

      - name: AWS IAM Setting
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: react build
        run: |
          npm install
          npm run build

      - name: upload build file to S3
        run: aws s3 cp --recursive --region ${{ secrets.AWS_REGION }} build s3://${{ secrets.AWS_S3_BUCKET }}
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

      - name: CloudFront cache reset
        uses: chetan/invalidate-cloudfront-action@v2
        env:
          DISTRIBUTION: ${{ secrets.AWS_CLOUDFRONT_ID }}
          PATHS: '/*'
          AWS_REGION: ${{ secrets.AWS_REGION }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

