---
title: "Spring DATA JPA in MySQL에서 Pessimistic Lock(비관적 락) 처리"
date: 2021-02-03
categories:
  - dev
tags:
  - jpa
  - PESSIMISTIC_WRITE
  - LockModeType
---

![](/img/title_jpa.png)


WMS운영 중 비정상적인 락 처리 현상이 있어 확인한 결과 공유드립니다.
Spring DATA JPA in MySQL에서 Pessimistic Lock(비관적 락) 처리    

- 락 어논테이션으로 처리 : @Lock(LockModeType.PESSIMISTIC_WRITE)
- 쓰기,읽기 모두 lock를 거는 방식이지만 실제로는 읽기에 대핸 lock이 걸리지 않음
- 읽기도 lock을 걸려면, 해당 row에 접근하는 select 쿼리에 모두 for update구문을 걸어줘야 함(@Lock(LockModeType.PESSIMISTIC_WRITE))
- 해당 락은 row level lock이지만 mysql innoDB의 락 처리 방식은 인덱싱 되지 않은 조건으로 선별 된 row에 대해선 row level로 락은 잡지 못함 
- mysql innoDB는 인덱싱 범위에 대해서 락을 잡는 방식으로 인덱싱 되지 않은 조건으로 선별 된 select결과는 row가 아닌 테이블이 락이 걸리는 현상이 발생됨
- 락을 걸어야 하는 데이터 셋의 검색조건은 인덱싱된 컬럼을 기준으로 잡아야 함
- JPA에서 lock scope범위에 따라 릴레이션 된 모델들도 함께 락이 잡히게 됨
- 대상 테이블만 락의 범위로 잡으려면 scope를 지정해줘야 함
- 락 범위 어논테이션으로 처리 : @QueryHints(value  = {@QueryHint(name  =  "javax.persistence.lock.scope", value  =  "NORMAL")})
