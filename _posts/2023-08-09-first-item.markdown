---
layout: post
title: "글 올리는 법"
subtitle: "jekyll 블로그 글 올리는 법"
date: 2023-08-09 12:00:00
author: "김우원"
catalog: true
tags:
  - jekyll
  - blog

---

## 글 작성

1. 마크다운 문법으로 블로그 글을 작성합니다.

   > 참고: https://www.markdownguide.org/basic-syntax/

2. 글을 작성한 후, 2가지 규칙을 적용해야합니다.

   - 파일의 확장자는 .md 또는 .markdown이여야 합니다.
   - 파일의 제목은 YYYY-MM-DD-[title]로 시작해야합니다.
     - ex) 2023-08-09-first-item
     - 파일명이 겹치지 않게 주의해 주세요
   - Markdown 시작부분에 다음과 같은 메타데이터를 첨부해야 합니다.

   ````
   ---
   layout: post 
   title: "글 올리는 법"
   subtitle: "jekyll 블로그 글 올리는 법"
   date: 2023-08-09 12:00:00
   author: "김우원"
   catalog: true
   tags:
     - jekyll
     - blog
   ---
   ````

   layout: post는 고정입니다. 수정하지 마세요

   title: 제목

   subtitle: 부제목

   date: YYYY-MM-DD 형식을 지켜주세요 시간은 선택입니다.

   author: 본인 이름을 입력해 주세요

   catalog: true시 toc(table of content)가 생깁니다.

   tags: 해시태그를 설정할 수 있습니다. `소문자만 사용해 주세요`

3. 이미지를 등록할 경우, 사용하는 이미지는 img 폴더에 저장해 주세요

## 글 등록

두가지 방법이 있습니다.

1. [bone-stew.github.io](https://github.com/bone-stew/bone-stew.github.io) 레포지토리를 클론 받아주세요.
   1. _post 폴더에 생성한 마크다운 파일을 옮기고 main브랜치로 push하면 됩니다.
2. git-hub 레포지토리에서 _post폴더에 바로 업로드 하셔도 됩니다.
