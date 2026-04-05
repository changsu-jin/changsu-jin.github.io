---
title: "Hybris Impex KeyGenerator 활용"
date: 2020-08-01
draft: true
categories:
  - dev
tags:
  - hybris
  - impex
---

customerGroup 생성시점은 모르겠는데, 일단 user모델 하위의 customer, employee 의 디폴트 그룹 데이터는 여기에서 관리 되는 것 같습니다.

```sql
select b.INTERNALCODE, A.P_EXTENSIONNAME, A.QUALIFIERINTERNAL, A.P_DEFAULTVALUE
from kicsm.ATTRIBUTEDESCRIPTORS a
    ,kicsm.COMPOSEDTYPES b
where 1=1
  and a.OWNERPKSTRING = b.pk
  and P_RELATIONNAME = 'PrincipalGroupRelation'
;
```

impex수행기록은 cronjob모델과 media모델 조합하면 기록은 다 남아 있는 것 같습니다. 수행시의 세션유저도 저장이 되고, 미디어파일 받아보면 처리 하려 했던 혹은 처리한 impex문장도 그대로 저장되어 있습니다.

## Hybris에서 impex를 이용하여 키생성해야 하는 경우

Hybris에서는 Oracle의 sequence나 MySQL에 autoIncrement와 같은 DB단의 증가값을 이용하지 않습니다.
Solution 자체적으로 KeyGenerator를 가지고 일정하게 증가하는 시퀀스를 사용합니다.

Impex에서 Translator를 사용하는 방법은 아래와 같습니다.

```
INSERT_UPDATE Customer[impex.legacy.mode=true]
;uid[unique=true];name[translator=com.kln.estore.core.impex.translators.SaleIdKeyGeneratorTranslator]
;;abcdefgh1;
;;abcdefgh2;
```

만든 Translator를 적용하기를 원하는 필드에 지정하면 impex에서 데이터를 추가할 때 Translator가 그 값으로 필드를 채우도록 되어 있습니다.
