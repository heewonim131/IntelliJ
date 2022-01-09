# 요구사항 정리

### 기능명세
||필요기능|
|:---:|:---|
|1|todo 리스트 목록에 아이템을 추가|
|2|todo 리스트 전체 목록을 조회|
|3|todo 리스트 목록 중 특정 아이템을 조회|
|4|todo 리스트 목록 중 특정 아이템을 수정|
|5|todo 리스트 목록 중 특정 아이템을 삭제|
|6|todo 리스트 전체 목록을 삭제|

### API 스펙
|method|endpoint|기능|request|response|
|---|---|---|---|---|
|POST|/|todo 아이템 추가|{<br>"title": "자료구조 공부하기"<br>}|{<br>"id": 17,<br>"title": "자료구조 공부하기",<br>"order": 0,<br>"completed": false,<br>"url": "http://localhost:8080/17"<br>}|
|GET|/|전체 todo 리스트 조회||{<br>"id": 1,<br>"title": "자바 기초 공부하기",<br>"order": 0,<br>"completed": false,<br>"url": "http://localhost:8080/1"<br>},<br>{<br>"id": 2,<br>"title": "알고리즘 공부하기",<br>"order": 0,<br>"completed": false,<br>"url": "http://localhost:8080/2"<br>}, ...|
|GET|/{:id}|todo 아이템 조회||{<br>"id": 17,<br>"title": "자료구조 공부하기",<br>"order": 0,<br>"completed": false,<br>"url": "http://localhost:8080/17"<br>}|
|PATCH|/{:id}|todo 아이템 수정|{<br>"title": "반복문 공부하기"<br>}|{<br>"id": 1,<br>"title": "반복문 공부하기",<br>"order": 0,<br>"completed": false,<br>"url": "http://localhost:8080/1"<br>}|
|DELETE|/{:id}|todo 아이템 삭제||200|
|DELETE|/|전체 todo 리스트 삭제||200|

https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C
https://www.todobackend.com
https://www.todobackend.com/specs/index.html
https://www.todobackend.com/client/index.html?http://todobackend-aiohttp.herokuapp.com
