---
title: "Private Docker Registry 구축"
date: 2020-08-01
draft: true
categories:
  - env
tags:
  - docker
  - registry
---

제 윈도우pc wsl 로컬에 있던 개발용 컨테이너나 도커이미지 맥북으로 옮길려고 docker private registry 하나 만들었습니다.(간단히 이미지 관리도 할겸..)

※ docker 설정파일에 아래 추가 후 리스타트

```json
"insecure-registries": [
  "172.18.124.149:5000"
]
```

※ 옮길 이미지 태깅 후 푸시/풀

```bash
docker push 172.18.124.149:5000/devbox-wms-web:1
```

※ private registry에 등록된 이미지 목록 조회

```bash
curl -X GET http://172.18.124.149:5000/v2/_catalog
```

※ 이미지별 태그 확인

```bash
curl -X GET http://172.18.124.149:5000/v2/redis/tags/list
```
