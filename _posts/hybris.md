customerGroup 생성시점은 모르겠는데, 일단 user모델 하위의 customer, employee 의 디폴트 그룹 데이터는 여기에서 관리 되는 것 같습니다.

select b.INTERNALCODE, A.P_EXTENSIONNAME, A.QUALIFIERINTERNAL, A.P_DEFAULTVALUE

from kicsm.ATTRIBUTEDESCRIPTORS a

,kicsm.COMPOSEDTYPES b

where 1=1

and a.OWNERPKSTRING = b.pk

and P_RELATIONNAME = 'PrincipalGroupRelation'

;









impex수행기록은 cronjob모델과 media모델 조합하면 기록은 다 남아 있는 것 같습니다. 수행시의 세션유저도 저장이 되고, 미디어파일 받아보면 처리 하려 했던 혹은 처리한 impex문장도 그대로 저장되어 있습니다.

성능이나 계정부분은 좀 더 검토해봐야겠지만

이 부분 활용하여 hac에 각 개인계정으로 로그인하게 해서 impex이력관리 가능할 것으로 보입니다.

select cj.P_CODE "impex수행cronjobcode"

--,e1.code "ㅁㅁㅁ"

,e2.code "현재상태"

,e3.code "결과"

,nvl(u.p_uid,emp.p_uid) "처리자ID"

,nvl(u.p_name,emp.p_name) "처리자명"

,m1.P_LOCATION "수행IMPEX파일경로"

,m1.pk "수행IMPEX파일PK"

,m3.P_LOCATION "처리csv파일경로"

,m3.pk "처리csv파일PK"

,m2.P_LOCATION "문제발생csv파일경로"

,m2.pk "문제발생csv파일PK"

from kicsm.CRONJOBS cj

,kicsm.medias m1

,kicsm.JOBS j

--,kicsm.ENUMERATIONVALUES e1

,kicsm.ENUMERATIONVALUES e2

,kicsm.ENUMERATIONVALUES e3

,kicsm.users u

,kicsm.medias m2

,kicsm.medias m3

,kicsm.EMPLOYEES emp

where 1=1

and cj.P_JOBMEDIA = m1.pk

--and cj.P_ERRORMODE = e1.pk

and cj.P_STATUS = e2.pk

and cj.P_RESULT = e3.pk

and u.pk(+) = cj.P_SESSIONUSER

and emp.pk(+) = cj.P_SESSIONUSER

and cj.P_JOB = j.pk

and cj.P_UNRESOLVEDDATASTORE = m2.pk(+)

and cj.P_WORKMEDIA = m3.pk(+)

and j.P_CODE = 'ImpEx-Import'

and cj.CREATEDTS > to_date('201906030000','YYYYMMDDHH24MISS')

;









## Hybris에서 impex를 이용하여 키생성해야 하는 경우 아래와 같은 방법으로 가능합니다.

## 업무에 참고 하시기 바랍니다.

Impex에서 keyGenerator를 이용해서 값을 처리하는 방법중에 Translator를 사용하는 방법이 있습니다.

 Impex에서 Translator를 사용하는 방법은 아래와 같습니다.INSERT_UPDATE Customer[impex.legacy.mode=true];uid[unique=true];name[translator=com.kln.estore.core.impex.translators.SaleIdKeyGeneratorTranslator];;abcdefgh1;;;abcdefgh2;; 만든 Translator를 적용하기를 원하는 필드에 지정하면 impex에서 데이터를 추가할 때 Translator가 그 값으로 필드를 채우도록 되어 있고 샘플은 Customer에 데이터를 넣는 예제입니다. Impex가 multi-thread로 동작할 때 키는 unique하게 생성되지만 나열한 데이터와 키의 순서는 현재는 보장되지 않습니다.  위에 데이터가 들어가면 아래와 같이 데이터가 입력이 됩니다. 참고 바랍니다.uid:abcdefgh1    name:000050000005  필요한 Translator가 있으면 아래 소스와 같은 방법으로 구현하시면 됩니다.package com.kln.estore.core.impex.translators;
import de.hybris.platform.core.Registry;import de.hybris.platform.impex.jalo.translators.AbstractValueTranslator;import de.hybris.platform.jalo.Item;import de.hybris.platform.jalo.JaloInvalidParameterException;import de.hybris.platform.servicelayer.keygenerator.impl.PersistentKeyGenerator;
public class SaleIdKeyGeneratorTranslator extends AbstractValueTranslator {
private PersistentKeyGenerator saleIdGenerator;
public PersistentKeyGenerator getSaleIdGenerator() {
if (saleIdGenerator == null) {
saleIdGenerator = Registry.getApplicationContext().getBean("kopSaleIdKeyGenerator", PersistentKeyGenerator.class);}
return saleIdGenerator;}
@Overridepublic Object importValue(String valueExpr, Item toItem) throws JaloInvalidParameterException {return (valueExpr != null && valueExpr.length() > 0) ? valueExpr : getSaleIdGenerator().generate();}
@Overridepublic String exportValue(Object value) throws JaloInvalidParameterException {return value == null ? null : value.toString();}}AbstractValueTranslator를 확장하고 importValue와 exportValue를 구현하면 됩니다. importValue는 impex import시에 exportValue는 impex export시에 호출이 됩니다. 위에 구현은 impex에 valueExpr이 있을때는 그 데이터를 사용하고 없으면 keyGenerator를 호출해서 데이터를 넣을 수 있도록 만든  Translator입니다.