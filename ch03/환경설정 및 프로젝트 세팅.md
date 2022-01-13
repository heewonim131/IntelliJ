# 환경설정 및 프로젝트 세팅

## 프로젝트 생성
1. New Project 를 눌러 프로젝트를 생성한다.<br>
<img src="https://user-images.githubusercontent.com/92259017/148679405-e7644f78-d06d-4f69-8f99-406ca910191b.png" style="width: 60%; height: 60%"/>

2. Gradle 과 java 체크 후 Next 클릭<br>

> 프로젝트 빌드 관리 툴로 메이븐 혹은 그래들 을 사용하는데, 사용 목적은 비슷하지만 사용성, 성능에 차이가 있다.<br>
좀 더 나중에 나온 그래들이 메이븐의 단점을 보완하기 때문에 이 프로젝트에서는 관리 툴로 그래들을 선택하였다.<br>

<img src="https://user-images.githubusercontent.com/92259017/148679583-c2d33ed4-d41a-457f-aaf3-7585ae493bab.png" style="width: 60%; height: 60%"/>

3. 프로젝트명을 입력하고 Finish 클릭<br>
<img src="https://user-images.githubusercontent.com/92259017/148679605-f918520c-041a-4faa-9245-d975c8a8c462.png" style="width: 60%; height: 60%"/>

4. BUILD SUCCESSFUL in ns 라는 메시지가 뜨면 프로젝트 빌드가 완료된 것이다.<br>

  > 프로젝트가 빌드되면 메인에 build.gradle 이라는 파일이 보이는데, 빌드에 필요한 옵션들을 정의하는 파일이다.<br>
build.gradle 에 작성된 빌드 구성을 기반으로 빌드 기능을 수행하게 된다.<br>

## 라이브러리 추가
기존에 있던 의존성은 모두 지우고, 필요한 것들을 하나씩 추가해주겠다<br>
<img src="https://user-images.githubusercontent.com/92259017/148933931-9ca0706a-6162-4f29-8613-92f3f6152b89.png" style="width: 60%; height: 60%"/>

- 스프링 부트 플러그인 추가<br>
<img src="https://user-images.githubusercontent.com/92259017/148934660-a34d29ef-e2ab-4782-9745-48d5897b46c3.png" style="width: 60%; height: 60%"/>

- REST, JPA, h2 데이터베이스 라이브러리 추가<br>
<img src="https://user-images.githubusercontent.com/92259017/148936601-3f3de285-068d-4e55-9cd7-7b0dd6104730.png" style="width: 60%; height: 60%"/>

- lombok 라이브러리 추가<br>
<img src="https://user-images.githubusercontent.com/92259017/148942171-1f8ae1f0-e34c-41df-8b50-b0e22acf71f3.png" style="width: 60%; height: 60%"/>

- Plugins 탭에서 lombok install<br>
<img src="https://user-images.githubusercontent.com/92259017/148939834-148d46cd-0483-467f-9b7c-560072f40307.png" style="width: 60%; height: 60%"/>

- Annotation Processors 탭에서 Annotation 허용<br>
<img src="https://user-images.githubusercontent.com/92259017/148939673-193d436b-8082-4082-b0af-eaae4ae9b72c.png" style="width: 60%; height: 60%"/>

## 자바 파일 테스트
- 테스트를 위해 org.example 패키지에 TodoServerApplication 클래스 파일 추가<br>
<img src="https://user-images.githubusercontent.com/92259017/148942704-12482f3c-8317-4050-88a2-da91a749e54f.png" style="width: 70%; height: 70%"/>
<img src="https://user-images.githubusercontent.com/92259017/148946806-d6f5b9ee-90e9-469c-83c0-15bbfbd39225.png" style="width: 70%; height: 70%"/>

## 참고자료
- [IntelliJ + Github 연동하기 by 갓대희](https://goddaehee.tistory.com/249)
