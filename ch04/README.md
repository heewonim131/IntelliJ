# 👾 디버깅

컴퓨터 프로그래밍 개발 단계중에 발생하는 시스템의 논리적인 오류나 비정상적인 버그를 찾아내고, 그 원인을 밝히고 수정하는 작업 과정.

## 디버깅 설정
### ![image](https://user-images.githubusercontent.com/92259017/149453015-44721819-397f-43d9-9113-50a0f843856f.png) Step Over
함수 내부까지 들어가지 않고 실행한 후 다음 라인으로 이동한다.

![image](https://user-images.githubusercontent.com/92259017/149451587-87a78b7b-41fd-4b50-892d-1bd21de79e70.png)

### ![image](https://user-images.githubusercontent.com/92259017/149453079-dbc962dc-d270-4af8-9af4-48b0a3dd4efb.png) Step Into
해당 라인의 함수 내부(선언부)로 들어가서 함수 내용을 한 줄씩 진행한다.

![image](https://user-images.githubusercontent.com/92259017/149451693-89e17dc2-cf95-42ce-8860-8989a1f5a9c0.png)
![image](https://user-images.githubusercontent.com/92259017/149451803-153932c0-ba20-4799-b77f-58e297737e46.png)

### ![image](https://user-images.githubusercontent.com/92259017/149453127-570a56fc-fc8c-444d-be59-9913814603ad.png) Force Step Into
Step Into로는 들어가지 못하는 메서드 내부까지 강제로 들어가 실행한다.

### ![image](https://user-images.githubusercontent.com/92259017/149453161-d085f49c-c774-4178-be55-946b96d85f55.png) Step Out
현재 실행중인(내부로 들어온) 함수의 남은 부분을 실행시키고 호출부로 되돌아 간다.

![image](https://user-images.githubusercontent.com/92259017/149451857-37504813-2267-4be3-ae62-88b3115defdc.png)
![image](https://user-images.githubusercontent.com/92259017/149451693-89e17dc2-cf95-42ce-8860-8989a1f5a9c0.png)

### ![image](https://user-images.githubusercontent.com/92259017/149453185-b0c45dc3-9247-47ef-9579-73dedf3a54ba.png) Drop Frame
현재 실행중인 함수의 남은 부분을 실행하지 않고 호출부로 되돌아 간다.

![image](https://user-images.githubusercontent.com/92259017/149451880-a3abf921-556a-4e3e-8e48-62d3fc07b6f0.png)
![image](https://user-images.githubusercontent.com/92259017/149451693-89e17dc2-cf95-42ce-8860-8989a1f5a9c0.png)

## 참고자료
- [디버깅 용어 질문](https://okky.kr/article/605812)
