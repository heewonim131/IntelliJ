# 모델 구현

다음과 같이 model 패키지에 TodoEntity, TodoRequest, TodoResponse 클래스를 생성한다.<br><br>
![image](https://user-images.githubusercontent.com/92259017/149070332-8b480156-9d04-4d88-bdab-8bd040936d00.png)

## TodoEntity
데이터베이스와 데이터를 주고 받기 위한 클래스<br>
Annotation을 이용해서 기본키, 널 허용 여부 등을 설정해준다.<br>
order 같은 경우 DB에서 예약어로 쓰이기 때문에 별도로 컬럼명(todoOrder)을 추가해 주었다.<br>
```
package org.example.model;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Data
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class TodoEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String title;

    @Column(name = "todoOrder", nullable = false)
    private Long order;

    @Column(nullable = false)
    private Boolean completed;
}
```

## TodoRequest
요청을 받아오기 위해 필요한 클래스.
```
package org.example.model;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class TodoRequest {

    private String title;
    private Long order;
    private Boolean completed;

}
```

## TodoResponse
요청에 응답하기 위해 필요한 클래스.<br>
응답할 때 Entity의 모든 값을 정의해주어야 하기 떄문에 Entity의 모든 변수들을 선언한다.<br>
요청을 편리하게 처리하기 위해 TodoEntity를 파라미터로 받는 생성자를 추가한다.

  > 아래 코드에서 url 변수를 'http://localhost:8080/'과 같이 정의하는 것은 좋은 방법은 아니다.<br>
url이나 port가 변경되는 경우 코드를 수정해야 하는 번거로움 때문에, 보통은 config나 properties를 통해 관리해야 하지만
이 프로젝트는 간단한 To do list를 만드는 것이 목적이기 때문에 작업을 늘리지 않고 url을 코드 내에 정의해주었다.

```
package org.example.model;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class TodoResponse {

    private Long id;
    private String title;
    private Long order;
    private Boolean completed;
    private String url;

    public TodoResponse(TodoEntity todoEntity) {
        this.id = todoEntity.getId();
        this.title = todoEntity.getTitle();
        this.order = todoEntity.getOrder();
        this.completed = todoEntity.getCompleted();

        this.url = "http://localhost:8080/" + this.id;
    }
}
```
