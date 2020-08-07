# Tip



## Mac

### Dock에 빈공간 삽입

```shell
defaults write com.apple.dock persistent-apps -array-add '{tile-data={}; tile-type="spacer-tile";}'; killall Dock
```

## Shell

### NVM

- [nvm 과 npm 구별하기](https://lynmp.com/ko/article/tb585d114096490055)
- [NVM으로 노드 버전 관리하기](http://jeonghwan-kim.github.io/2016/08/10/nvm.html) 
- .zshrc 참고


```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
```

### SDKMAN

- 설치

```shell
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java 8.0.252.hs-adpt
```

- .zshrc 참고

```shell
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="/Users/purple/.sdkman"
[[ -s "/Users/purple/.sdkman/bin/sdkman-init.sh" ]] && source "/Users/purple/.sdkman/bin/sdkman-init.sh"
```

### TMUX

- [우분투(Ubuntu)에 tmux 설치/세팅하기](https://seongkyun.github.io/others/2019/01/05/tmux/)

### GIT

- git 작업내용 되돌리기

```shell
git clean -f
git reset --hard
git stash -u
```

- Git remote repository에 push를 할 때 커밋 히스토리에 Merge branch 'master' of http://.... 가 생기는 경우에 대해서 문의가 있어서 남깁니다.

  [GitHub Merge branch 'master'](http://stackoverflow.com/questions/7120199/github-merge-branch-master)

  두개의 브랜치 A와 B가 동일한 상태에서 A가 push를 하고 B가 remote repository sync없이 다른 작업을 push하는 경우 reject가 나고, 이때 pull을 받는 과정에서 remote의 commit이 merge가 되는데 이 merge건에 대한 commit history입니다.



### GRADLE

- 디펜던시 리로드

```shell
./gradlew --refresh-dependencies --info
```

- 리퀴베이스(변경로그 추출 / 체크섬 클리어 / 롤백)

```shell
./gradlew bootjar -x test  ## 패키지화
./gradlew liquibaseDiffChangelog -PrunList=diffLog  ## 패키지된 소스와 현재DB와의 차이점 추출

./gradlew liquibaseClearChecksums ## 체크섬 클리어

./gradlew liquibaseRollbackCount -PliquibaseCommandValue=11 ## 롤백
```

### NPM

- project install 오류 시

```shell
npm cache clean -f
rm -rf node_modules/
rm package-lock.json
```

- 프록시 세팅

```shell
npm config set https-proxy http://211.251.239.13:8118
```

- node_module/.bin 심볼릭링크 문제

```shell
npm install --no-bin-links
```

### DOCKER

- 설치

```shell
https://docs.docker.com/docker-for-windows/install/curl -fsSL https://get.docker.com/ | sudo sh
sudo usermod -aG docker $USER
sudo apt-get install docker-compose
export DOCKER_HOST=tcp://127.0.0.1:2375
```

###  WSL

- 시작/중지 (in powershell)

```powershell
net stop LxssManager
net start LxssManager
```

- Mount root 변경(wsl1)

```shell
$ sudo nano /etc/wsl.conf
```

```properties
[automount]
root = /
options = "metadata"
```

- DOCKER FOR WINDOWS + WSL(WINDOWS SUBSYSTEM LINUX) 볼륨설정 관련
  - Docker for Windows+WSL 에서 Volumn 마운트 경로는 wsl에서 /c, /d 로 시작되는 경로만 가능하다.
  - WSL은 경로구성이 /mnt/c, /mnt/d 로 기본으로 잡혀 있으며, 해당경로는 도커에서 인지하지 못한다.
  - 도커에서 WSL경로를 볼륨으로 잡기 위해선, wsl.conf 파일을 생성하여 wsl mount경로를 바인딩해주어야 한다.(/mnt/c -> /c) 
    (윈도우 빌드버전 18.03 이상인 경우, 이하 버전은 wsl에서 mount명령어로 직접 바인딩 후 ~/.bashrc 파일에 설정해줘야 함)
  - Docker for Windows -> Resources -> FILE SHARING메뉴에서 C 체크해줘야 함

- WSL HOST의TIMEZONE 파일을 VOLUME으로 잡고, 도커컨테이너에서 HOST와 TIMEZONE 싱크하는 예제

```shell
[WSL console]
cp /etc/timezone /c/volumn/timezone (심볼릭링크는 못찾더군요..)

[Docker compose  : mysql.yml]
....
volumes:
\- "/c/volumn/timezone:/etc/timezone:ro"  
environment:
\- TZ=Asia/Seoul
....

[WSL console]
docker-compose -f mysql.yml up -d
docker exec -it <컨테이너ID> /bin/bash

[Docker container]
date   ==> host의 시간과 일치하는 지 확인
cat /etc/timezone ==> host의 타임존과 일치하는 지 확인

```

```mysql
[Docker container : mysql console]
mysql -u root
select now();
SELECT @@GLOBAL.time_zone, @@SESSION.time_zone, @@system_time_zone;
```

  

- 참고링크
  - https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly#ensure-volume-mounts-work
  - https://stackoverflow.com/questions/59942110/docker-drive-has-not-been-shared





### JHipster

- 최초생성

```shell
jhipster --skip-cache --skip-install --skip-git --jhi-prefix=csj
```

- prefix변경(regenerator-소스코드에서 변경한 경우)

```shell
 jhipster --with-entities --skip-install
```

### KU8

- minikube
```shell
sudo apt-get install -y conntrack
sudo -E minikube start --vm-driver=none --extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
```



### MS Terminal

- https://docs.microsoft.com/ko-kr/windows/terminal/panes

### Ruby

- 설치

```shell
sudo apt install ruby-full build-essential zlib1g-dev
```

- sudo 없이 gem 쓰기 위해 gem 패키지가 생성될 폴더 만들기

```shell
mkdir .gems
# Install Ruby Gems to ~/gems
export GEM_HOME="$HOME/.gems"
export PATH="$HOME/.gems/bin:$PATH"
```

- Jekyll과 Bundler 설치

```shell
gem install jekyll bundler
```

- Theme에 따른 의존성 설치

```shell
bundle install
```

## Spring JPA

### Supported keywords inside method names

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/springjpa.png)


###  JPA N+1튜닝과정에서 선언한 배치 사이즈와 다르게 쿼리 분할 되어 수행되는 현상공유

먼저 이 현상을 설명하려면 JDBC의 preparedstatement의 캐싱 방식을 알아야 합니다.
preparedstatement는 in절이 들어가는 select 쿼리에 대해 각 경우를 모두 캐싱합니다.
- 데이터가 1개 들어올 때 : where xxx in (?)
- 데이터가 2개 들어올 때 : where xxx in (?,?) 
- 데이터가 n개 들어올 때 : where xxx in (?,?, ...) 

이렇게 캐시하는 경우 데이터가 많아지면 너무 많은 케이스를 캐싱해야 하고 성능에 문제가 발생하므로 하이버네이트는 최적화를 위해 캐싱케이스를 줄입니다.

줄이는 방식은  프로젝트에 선언 된 기본배치사이즈(hibernate.default_batch_fetch_size:  100)를 기준으로 절반씩 나눠가면서 캐싱합니다.
그리고 자주사용할 것으로 예상되는 1~10 사이는 모두 캐싱 하게되며, 기준값을 100으로 잡았을 때, 기존 preparedstatement 방식에 의하면 총 100개의 케이스를 캐싱해두게 되지만, 하이버네이트의 방식으로 하게 되면 14개로 줄어듭니다.(1,2,3,4,5,6,7,8,9,10,12, 25,50,100)

N+1이슈가 발생한 쿼리가 있고 총 데이터가 83건이면 83번의 동일한 쿼리가 나가는 상황일겁니다.
이 현상의 튜닝을 위해 hibernate.default_batch_fetch_size:  100 으로 잡고 데이터를 조회하면, 쿼리가 한번 나가고 in절 항목이 83개가 들어가게 될 것으로 예상되지만 실제로는 아래와 같이 총 3번의 동일한 쿼리가 나가게 됩니다. 

- in절 항목이 50으로 캐싱된 쿼리 한 번
- in절 항목이 25로 캐싱된 쿼리 한 번
- in절 항목이 8으로 캐싱된 쿼리 한 번 
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

이는 하이버네이트 최적화 전략에 의해, 정상적인 부분이며 권장하는 기본전략입니다.
참고로, 선언한(hibernate.default_batch_fetch_size:  100 ) 사이즈 만큼 in절 항목을 발생시키고 싶다면 설정파일에 ```hibernate.batch_fetch_style: dynamic ```을 추가하시면 됩니다.(캐싱되지 않은 케이스로 쿼리가 수행되므로 권장하지 않는 방식이라고 합니다.)




## IDE Tool

### VSCODE

- 항상 새탭으로 열리게

```properties
workbench.editor.enablePreview
```

- Language Support for Java (서버 크래쉬 나는 경우) setting.json에 java.jdt.ls.vmargs값에 -DwatchParentProcess=false 추가

```properties
"java.jdt.ls.vmargs": "-DwatchParentProcess=false -Dlog.level=ALL -XX:+UseParallelGC -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xmx1G -Xms100m",
```

- 다중행 커서를 마지막으로 : shift+alt+i

- JAVA 컴파일러 옵션

```properties
org.eclipse.jdt.core.compiler.annotation.inheritNullAnnotations=disabled
org.eclipse.jdt.core.compiler.annotation.missingNonNullByDefaultAnnotation=ignore
org.eclipse.jdt.core.compiler.annotation.nonnull=org.eclipse.jdt.annotation.NonNull
org.eclipse.jdt.core.compiler.annotation.nonnull.secondary=
org.eclipse.jdt.core.compiler.annotation.nonnullbydefault=org.eclipse.jdt.annotation.NonNullByDefault
org.eclipse.jdt.core.compiler.annotation.nonnullbydefault.secondary=
org.eclipse.jdt.core.compiler.annotation.nullable=org.eclipse.jdt.annotation.Nullable
org.eclipse.jdt.core.compiler.annotation.nullable.secondary=
org.eclipse.jdt.core.compiler.annotation.nullanalysis=disabled
org.eclipse.jdt.core.compiler.codegen.inlineJsrBytecode=enabled
org.eclipse.jdt.core.compiler.codegen.methodParameters=do not generate
org.eclipse.jdt.core.compiler.codegen.unusedLocal=preserve
org.eclipse.jdt.core.compiler.debug.lineNumber=generate
org.eclipse.jdt.core.compiler.debug.localVariable=generate
org.eclipse.jdt.core.compiler.debug.sourceFile=generate
org.eclipse.jdt.core.compiler.problem.APILeak=warning
org.eclipse.jdt.core.compiler.problem.annotationSuperInterface=warning
org.eclipse.jdt.core.compiler.problem.assertIdentifier=error
org.eclipse.jdt.core.compiler.problem.autoboxing=ignore
org.eclipse.jdt.core.compiler.problem.comparingIdentical=warning
org.eclipse.jdt.core.compiler.problem.deadCode=warning
org.eclipse.jdt.core.compiler.problem.deprecation=warning
org.eclipse.jdt.core.compiler.problem.deprecationInDeprecatedCode=disabled
org.eclipse.jdt.core.compiler.problem.deprecationWhenOverridingDeprecatedMethod=disabled
org.eclipse.jdt.core.compiler.problem.discouragedReference=warning
org.eclipse.jdt.core.compiler.problem.emptyStatement=ignore
org.eclipse.jdt.core.compiler.problem.enumIdentifier=error
org.eclipse.jdt.core.compiler.problem.explicitlyClosedAutoCloseable=ignore
org.eclipse.jdt.core.compiler.problem.fallthroughCase=ignore
org.eclipse.jdt.core.compiler.problem.fatalOptionalError=disabled
org.eclipse.jdt.core.compiler.problem.fieldHiding=ignore
org.eclipse.jdt.core.compiler.problem.finalParameterBound=warning
org.eclipse.jdt.core.compiler.problem.finallyBlockNotCompletingNormally=warning
org.eclipse.jdt.core.compiler.problem.forbiddenReference=warning
org.eclipse.jdt.core.compiler.problem.hiddenCatchBlock=warning
org.eclipse.jdt.core.compiler.problem.includeNullInfoFromAsserts=disabled
org.eclipse.jdt.core.compiler.problem.incompatibleNonInheritedInterfaceMethod=warning
org.eclipse.jdt.core.compiler.problem.incompleteEnumSwitch=warning
org.eclipse.jdt.core.compiler.problem.indirectStaticAccess=ignore
org.eclipse.jdt.core.compiler.problem.localVariableHiding=ignore
org.eclipse.jdt.core.compiler.problem.methodWithConstructorName=warning
org.eclipse.jdt.core.compiler.problem.missingDefaultCase=ignore
org.eclipse.jdt.core.compiler.problem.missingDeprecatedAnnotation=ignore
org.eclipse.jdt.core.compiler.problem.missingEnumCaseDespiteDefault=disabled
org.eclipse.jdt.core.compiler.problem.missingHashCodeMethod=ignore
org.eclipse.jdt.core.compiler.problem.missingOverrideAnnotation=ignore
org.eclipse.jdt.core.compiler.problem.missingOverrideAnnotationForInterfaceMethodImplementation=enabled
org.eclipse.jdt.core.compiler.problem.missingSerialVersion=warning
org.eclipse.jdt.core.compiler.problem.missingSynchronizedOnInheritedMethod=ignore
org.eclipse.jdt.core.compiler.problem.noEffectAssignment=warning
org.eclipse.jdt.core.compiler.problem.noImplicitStringConversion=warning
org.eclipse.jdt.core.compiler.problem.nonExternalizedStringLiteral=ignore
org.eclipse.jdt.core.compiler.problem.nonnullParameterAnnotationDropped=warning
org.eclipse.jdt.core.compiler.problem.nonnullTypeVariableFromLegacyInvocation=warning
org.eclipse.jdt.core.compiler.problem.nullAnnotationInferenceConflict=error
org.eclipse.jdt.core.compiler.problem.nullReference=warning
org.eclipse.jdt.core.compiler.problem.nullSpecViolation=error
org.eclipse.jdt.core.compiler.problem.nullUncheckedConversion=warning
org.eclipse.jdt.core.compiler.problem.overridingPackageDefaultMethod=warning
org.eclipse.jdt.core.compiler.problem.parameterAssignment=ignore
org.eclipse.jdt.core.compiler.problem.pessimisticNullAnalysisForFreeTypeVariables=warning
org.eclipse.jdt.core.compiler.problem.possibleAccidentalBooleanAssignment=ignore
org.eclipse.jdt.core.compiler.problem.potentialNullReference=ignore
org.eclipse.jdt.core.compiler.problem.potentiallyUnclosedCloseable=ignore
org.eclipse.jdt.core.compiler.problem.rawTypeReference=warning
org.eclipse.jdt.core.compiler.problem.redundantNullAnnotation=warning
org.eclipse.jdt.core.compiler.problem.redundantNullCheck=ignore
org.eclipse.jdt.core.compiler.problem.redundantSpecificationOfTypeArguments=ignore
org.eclipse.jdt.core.compiler.problem.redundantSuperinterface=ignore
org.eclipse.jdt.core.compiler.problem.reportMethodCanBePotentiallyStatic=ignore
org.eclipse.jdt.core.compiler.problem.reportMethodCanBeStatic=ignore
org.eclipse.jdt.core.compiler.problem.specialParameterHidingField=disabled
org.eclipse.jdt.core.compiler.problem.staticAccessReceiver=warning
org.eclipse.jdt.core.compiler.problem.suppressOptionalErrors=disabled
org.eclipse.jdt.core.compiler.problem.suppressWarnings=enabled
org.eclipse.jdt.core.compiler.problem.syntacticNullAnalysisForFields=disabled
org.eclipse.jdt.core.compiler.problem.syntheticAccessEmulation=ignore
org.eclipse.jdt.core.compiler.problem.terminalDeprecation=warning
org.eclipse.jdt.core.compiler.problem.typeParameterHiding=warning
org.eclipse.jdt.core.compiler.problem.unavoidableGenericTypeProblems=enabled
org.eclipse.jdt.core.compiler.problem.uncheckedTypeOperation=warning
org.eclipse.jdt.core.compiler.problem.unclosedCloseable=warning
org.eclipse.jdt.core.compiler.problem.undocumentedEmptyBlock=ignore
org.eclipse.jdt.core.compiler.problem.unhandledWarningToken=warning
org.eclipse.jdt.core.compiler.problem.unlikelyCollectionMethodArgumentType=warning
org.eclipse.jdt.core.compiler.problem.unlikelyCollectionMethodArgumentTypeStrict=disabled
org.eclipse.jdt.core.compiler.problem.unlikelyEqualsArgumentType=info
org.eclipse.jdt.core.compiler.problem.unnecessaryElse=ignore
org.eclipse.jdt.core.compiler.problem.unnecessaryTypeCheck=ignore
org.eclipse.jdt.core.compiler.problem.unqualifiedFieldAccess=ignore
org.eclipse.jdt.core.compiler.problem.unusedDeclaredThrownException=ignore
org.eclipse.jdt.core.compiler.problem.unusedDeclaredThrownExceptionExemptExceptionAndThrowable=enabled
org.eclipse.jdt.core.compiler.problem.unusedDeclaredThrownExceptionIncludeDocCommentReference=enabled
org.eclipse.jdt.core.compiler.problem.unusedDeclaredThrownExceptionWhenOverriding=disabled
org.eclipse.jdt.core.compiler.problem.unusedExceptionParameter=ignore
org.eclipse.jdt.core.compiler.problem.unusedImport=warning
org.eclipse.jdt.core.compiler.problem.unusedLabel=warning
org.eclipse.jdt.core.compiler.problem.unusedLocal=warning
org.eclipse.jdt.core.compiler.problem.unusedObjectAllocation=ignore
org.eclipse.jdt.core.compiler.problem.unusedParameter=ignore
org.eclipse.jdt.core.compiler.problem.unusedParameterIncludeDocCommentReference=enabled
org.eclipse.jdt.core.compiler.problem.unusedParameterWhenImplementingAbstract=disabled
org.eclipse.jdt.core.compiler.problem.unusedParameterWhenOverridingConcrete=disabled
org.eclipse.jdt.core.compiler.problem.unusedPrivateMember=warning
org.eclipse.jdt.core.compiler.problem.unusedTypeParameter=ignore
org.eclipse.jdt.core.compiler.problem.unusedWarningToken=warning
org.eclipse.jdt.core.compiler.problem.varargsArgumentNeedCast=warning
```

- 접속된 remote별로 theme 설정 (설정에서 조회해서 들어가야 사용가능한 theme을 볼 수 있음)

```properties
{
    "workbench.colorTheme": "<your theme of choice>"
}
```



## DataBase

### MySQL

- FEDERATED Engine을 이용한 DB 링크 구현

```mysql
show engines;
```

![before](https://i.imgur.com/lBJf7Bz.png)

- /etc/mysql/my.cnf 에 FEDERATED Engine 사용등록

```bash
vi /etc/mysql/my.cnf
--------------------------------------------------
[mysqld]
federated
```

- MySQL 재시작

```bash
service mysql restart;
```

- FEDERATED Engine 활성화 확인

```mysql
show engines;
```

![after](https://i.imgur.com/gyPa2G8.png)

- FEDERATED Engine을 이용해서 연결대상 DB의 테이블과 동일한 테이블 생성해서 연결

~~~mysql
CREATE TABLE mig.product (
  `product_code` varchar(255) NOT NULL,
  `product_name` varchar(255) DEFAULT NULL,
  `brand_code` varchar(100) DEFAULT NULL,
  `product_type` varchar(100) DEFAULT NULL
) ENGINE=FEDERATED DEFAULT CHARSET=utf8
CONNECTION='mysql://mig:****@sta-wms-aurora-mysql-instance-n.clvlkkfuxme1.ap-northeast-2.rds.amazonaws.com:3306/mig/product'
;
~~~

## 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDg0MTIyNTksLTgyMDYzNDgzNF19
-->