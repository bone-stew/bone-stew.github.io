---
layout: post
title: "Redis를 이용한 조회수 기능 구현"
date: 2023-09-03 12:00:00
author: "조상원"
catalog: true
tags:
  - redis
  - spring
---

## **Redis를 이용한 조회수 기능 구현**
안녕하세요 조상원입니다. 안드로이드에서 사용자에게 보이는 추천 팝업스토어 목록을 구현하면서 적용했던 내용을 소개해드리겠습니다.

### 추천 팝업스토어 목록 구현
유저에게 추천 팝업스토어 목록을 제시하기 위해 팝업스토어별 조회수를 정렬 하기로 했습니다. 유저가 팝업스토어의 상세정보를 요청할 때 조회수를 업데이트해야 했고, 이 작업을 효율적으로 처리하기 위한 방법을 고민했습니다.

처음에는 쿠키와 세션을 고려했습니다. 유저가 포스트를 열 때마다 쿠키 또는 세션을 활용하여 조회수를 증가시키고 데이터베이스를 업데이트하는 방식을 생각했습니다. 하지만 이러한 방식은 성능 면에서 데이터베이스에 부하가 발생하고 응답 시간이 길어질 것으로 판단했습니다.

### 요구사항 분석
Redis를 사용하여 조회수를 관리하기로 결정했고 Redis를 사용하기 위한 요구사항을 정리해보았습니다.

- 특정 유저의 포스트 방문 여부를 추적합니다.
- 유저가 포스트를 방문할 해당 포스트의 조회수를 업데이트합니다.
- 업데이트 주기를 설정하여 데이터베이스와 Redis의 데이터 일관성을 유지합니다.

### 실행 결과
이러한 조건을 바탕으로 Redis의 키-값 구조를 이용하여 유저 아이디와 포스트 아이디를 합친 키를 이용해 방문 여부를 추적했습니다.

```java
if (userFirstTimeViewingPost(popupStoreId, userId)) {
    popupStoreRepository.createUserViewedKey(popupStoreId, userId);
    popupStoreRepository.incrementPostView(popupStoreId, popupStoreDetailDto.getPopupStore().getViews());
}
```

```java
private boolean userFirstTimeViewingPost(Long popupStoreId, Long userId) {
    Boolean keyExists = popupStoreRepository.hasKey(popupStoreId, userId);
    if (keyExists && keyExists != null) {
        return false;
    }
    return true;
}
```

비슷한 방식으로 유저가 포스트를 방문할때 포스트 아이디를 키값으로 이용해 조회수를 업데이트했습니다. 

```java
public void incrementPostView(Long popupStoreId, Long views) {
    String postKey = "POST:" + popupStoreId;
    Boolean keyExists = redisTemplate.hasKey(postKey);
    if (keyExists && keyExists != null) {
        redisTemplate.opsForValue().increment(postKey);
    } else {
        redisTemplate.opsForValue().set(postKey, views + 1L);
    }
}
```

이렇게 이용된 포스트별 키값과 방문 여부를 스케줄러를 이용해 하루에 한번씩 데이터베이스에 업데이트하고 키를 삭제하는 방식을 적용했습니다.
```java
@Scheduled(fixedRate = 24 * 60 * 60 * 1000)
  @Transactional
  public void updateRedisPopupStoreViews() {
      Set<String> redisKeys = popupStoreRepository.getKeys("POST:*");
      List<PopupStoreUpdateDto> updates = new ArrayList<>();
      if (redisKeys != null) {
          for (String key : redisKeys) {
              String[] parts = key.split(":");
              Long popupStoreId = Long.parseLong(parts[1]);
              Long views = popupStoreRepository.getViews(key);
              PopupStoreUpdateDto updateDto = new PopupStoreUpdateDto(popupStoreId, views);
              updates.add(updateDto);
              popupStoreRepository.removeKey(key);
          }
      }
      if (!updates.isEmpty()) {
          popupStoreDao.batchUpdatePopupStoreViews(updates);
      }
  }

```
