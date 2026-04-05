---
title: "JPA N+1튜닝과정에서 선언한 배치 사이즈와 다르게 쿼리 분할 되어 수행되는 이유"
date: 2021-02-01
categories:
  - dev
tags:
  - jpa
  - batch_size
  - n+1
image: "/img/title_jpa.png"
---

![](/img/title_jpa.png)



## JDBC preparedstatement의 캐싱 방식

먼저 이 현상을 설명하려면 JDBC의 preparedstatement의 캐싱 방식을 알아야 합니다.  
preparedstatement는 in절이 들어가는 select 쿼리에 대해 각 경우를 모두 캐싱합니다.

```sql
# 데이터가 1개 들어올 때 : where xxx in (?)
# 데이터가 2개 들어올 때 : where xxx in (?,?)
# 데이터가 n개 들어올 때 : where xxx in (?,?, ...)
```
## hibernate의 batch_size에 의한 캐싱 방식

위의 방식으로 캐시되는 경우 데이터가 많아질수록 많은 케이스를 캐싱해야 해서 성능에 문제가 발생합니다.  
그래서 하이버네이트는 최적화를 위해 캐싱케이스를 줄이는 방식으로 수행됩니다.  
줄이는 방식은 프로젝트에 선언 된 기본배치사이즈 *(hibernate.default_batch_fetch_size: 100)* 를 기준으로 절반씩 나눠가면서 캐싱합니다.  
그리고 자주사용할 것으로 예상되는 1~10 사이는 모두 캐싱 하게됩니다.  
in절 항목값을 100으로 잡을 때, 기존 preparedstatement 방식에 의하면 총 100개의 케이스를 캐싱해두게 됩니다.  
하이버네이트의 방식으로 하게 되면 14개로 줄어듭니다.  
(1,2,3,4,5,6,7,8,9,10,12, 25,50,100)  

N+1이슈가 발생한 쿼리가 있고 총 데이터가 83건이면 83번의 동일한 쿼리가 나가는 상황일겁니다.  
현상 튜닝을 위해 *hibernate.default_batch_fetch_size: 100* 으로 설정 후 조회합니다.  
예상으로는 쿼리는 한번 나가고 in절 항목이 83개가 들어간 쿼리를 기대합니다.  
하지만 실제로는 캐싱된 케이스에 의해 아래와 같이 총 3번의 동일한 쿼리가 나가게 됩니다.  

```
in절 항목이 50으로 캐싱된 쿼리 한 번
in절 항목이 25로 캐싱된 쿼리 한 번
in절 항목이 8으로 캐싱된 쿼리 한 번
```

## 83개 항목이 분리 요청 된 실제 예

```sql
 select
        product0_.id as id1_38_0_,
        product0_.created_by as created_2_38_0_,
        product0_.created_date as created_3_38_0_,
        product0_.last_modified_by as last_mod4_38_0_,
        product0_.last_modified_date as last_mod5_38_0_,
        product0_.code as code6_38_0_,
        product0_.name as name7_38_0_,
        product0_.product_group_code as product_8_38_0_,
        product0_.product_type as product_9_38_0_
    from
        product product0_
    where
        product0_.id in (
            ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?
        )
2020-08-07 17:33:30,948 DEBUG [XNIO-1 task-1] SQL:
    select
        product0_.id as id1_38_0_,
        product0_.created_by as created_2_38_0_,
        product0_.created_date as created_3_38_0_,
        product0_.last_modified_by as last_mod4_38_0_,
        product0_.last_modified_date as last_mod5_38_0_,
        product0_.code as code6_38_0_,
        product0_.name as name7_38_0_,
        product0_.product_group_code as product_8_38_0_,
        product0_.product_type as product_9_38_0_
    from
        product product0_
    where
        product0_.id in (
            ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?
        )
2020-08-07 17:33:30,969 DEBUG [XNIO-1 task-1] SQL:
    select
        product0_.id as id1_38_0_,
        product0_.created_by as created_2_38_0_,
        product0_.created_date as created_3_38_0_,
        product0_.last_modified_by as last_mod4_38_0_,
        product0_.last_modified_date as last_mod5_38_0_,
        product0_.code as code6_38_0_,
        product0_.name as name7_38_0_,
        product0_.product_group_code as product_8_38_0_,
        product0_.product_type as product_9_38_0_
    from
        product product0_
    where
        product0_.id in (
            ?, ?, ?, ?, ?, ?, ?, ?
        )
```        

## 최적화 전략
위와 같이 하이버네이트는 최적화 전략으로 수행됩니다.  
3번의 캐싱된 쿼리가 수행되는 것은 정상적인 부분이며 하이버네이트에서 권장하는 기본전략입니다.  
참고로, 선언한 *(hibernate.default_batch_fetch_size: 100 )* 사이즈 만큼 in절 항목을 발생시키고 싶다면 설정파일에 *hibernate.batch_fetch_style: dynamic*을 추가하시면 됩니다.  
하지만 이 방식은 캐싱되지 않은 케이스로 쿼리가 수행되므로 권장하지 않는 방식이라고 합니다.