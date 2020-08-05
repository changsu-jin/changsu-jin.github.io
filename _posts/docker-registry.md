제 윈도우pc wsl 로컬에 있던 개발용 컨테이너나 도커이미지 맥북으로 옮길려고 docker private registry 하나 만들었습니다.(간단히 이미지 관리도 할겸..)
혹시 필요하신 분은 같이 사용해도 될 거 같아서 공유드립니다. 사무실 코오롱망에서 접근 가능합니다.

※ docker 설정파일에 아래 추가 후 리스타트

"insecure-registries": [
"172.18.124.149:5000"
]

※ 옮길 이미지 태깅 후 푸시/풀

ex) docker push 172.18.124.149:5000/devbox-wms-web:1

※ private registry에 등록된 이미지 목록 조회
curl -X GET http://172.18.124.149:5000/v2/_catalog

※ 이미지별 태그 확인  
curl -X GET http://172.18.124.149:5000/v2/redis/tags/list
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDczODA2MDIyXX0=
-->