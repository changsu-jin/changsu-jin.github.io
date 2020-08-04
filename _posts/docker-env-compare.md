---
title:  "Docker 개발환경 성능 비교"
excerpt: "개발환경별로 도커 성능 테스트를 해보았다"
toc: true
toc_sticky: true 

categories:
  - env
tags:
  - docker
last_modified_at: 2019-04-13T08:06:00-05:00
typora-root-url: ../
---

![](https://i.imgur.com/NMwVYyE.jpg)

도커 성능 테스트라고는 하지만 각 개발환경 호스트의 파일시스템에 따른 도커볼륨 I/O 속도 테스트 인 것 같다.





# WSL + Docker Desktop for Windows

## eeee

### aaaa



이 환경에선 초반에 버그도 많았고, 사용하면서 많은 인내심이 필요했다.
(*작년쯤엔 wsl2테스트 해보려고 [윈도우 인사이더 프로그램으로 진행하다가 O/S](https://github.com/microsoft/WSL/issues/4978)도 한번 날려 먹고....*)

사용하다 보니 점차 안정화 되고, [Visual Studio Code에서 원격개발 Extension](https://code.visualstudio.com/docs/remote/containers)도 나오면서 속도가 좀 아쉽긴 했어도 그럭저럭 개발환경으로서 사용할만 했다.





# WSL2 + Docker Desktop for Windows

Pages가 다른 블로그 플랫폼 보다 편한 것 같아서 마음에 든다.
다른 사람들도 같이 많이 사용했으면 좋겠다는 생각이 든다.

YFM에서 정의한 제목을 이중 괄호 구문으로 본문에 추가할 수 있다.
이 글의 제목은 {{ page.title }}이고
마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.

# Mac OS + Docker Desktop for Mac



# Mac OS + Docker Desktop for Mac(edge)

![](https://i.imgur.com/MgxpQbQ.png)

![](https://i.imgur.com/nF9Aalx.png)





# Ubuntu 18.04













