# repository 구현

## repository 란?
데이터베이스와 데이터를 주고 받기 위한 인터페이스를 정의한 부분.<br>
실제 데이터를 저장하고 있는 클래스가 아니라, **데이터를 주고 받는 인터페이스** 라는 점을 기억해두자.

- repository 패키지에 TodoRepository 인터페이스를 생성한다.<br><br>
![image](https://user-images.githubusercontent.com/92259017/149074331-9e48bbf7-7db4-4324-b9e6-5f12f2dc3459.png)

## TodoRepository
JPA repository 인터페이스를 상속받아 작성한다.<br>
제네릭은 DB 테이블과 연결될 객체인 TodoEntity 타입, 해당 객체의 id 필드인 Long 타입으로 정의해준다.

```
package org.example.repository;

import org.example.model.TodoEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface TodoRepository extends JpaRepository<TodoEntity, Long> {
}
```

아무런 메서드도 정의해주지 않았지만, main()에서 객체를 선언해 보면
상속받은 JPA repository의 메서드를 사용할 수 있다는 것을 알 수 있다.<br><br>
![image](https://user-images.githubusercontent.com/92259017/149075901-0c4d1d5f-def1-4ebf-af52-012a9b74c035.png)
