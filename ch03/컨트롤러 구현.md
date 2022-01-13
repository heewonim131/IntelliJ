# 컨트롤러 구현

컨트롤러는 요청에 따라서 어떤 작업을 처리하고 응답을 내릴 지 구현하는 부분이다.<br>
1) 컨트롤러 뼈대를 먼저 구성한 뒤 구조가 작동하는 지를 우선 확인 후에,<br>
2) 컨트롤러 메서드 내용을 구현하고, Postman과 Todo 페이지에서 기능이 잘 작동하는지 테스트를 해 볼 것이다.

controller 패키지에 TodoController 클래스 파일을 생성한다.<br><br>
![image](https://user-images.githubusercontent.com/92259017/149123061-9b75f4d3-634f-44d8-94d8-717aeaef04f8.png)

# 1) 컨트롤러 구조 테스트
## TodoController
@RestController(컨트롤러 표시), @CrossOrigin(CORS 이슈 해결) 등 필요한 어노테이션을 추가한다.<br>
클래스 내부에는 들어오는 요청 별로 처리할 응답을 매핑한다.<br>
**실질적인 메서드 내용은 API 구조가 정상적으로 작동하는 지 확인 후에 구현할 것이다.**

<details>
    <summary>코드 보기</summary>
    
```
package org.example.controller;

import lombok.AllArgsConstructor;
import org.example.model.TodoResponse;
import org.example.service.TodoService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@CrossOrigin
@RestController
@AllArgsConstructor
@RequestMapping("/")
public class TodoController {

    private final TodoService service;

    @PostMapping
    public ResponseEntity<TodoResponse> create() {
        System.out.println("CREATE");
        return null;
    }

    @GetMapping
    public ResponseEntity<List<TodoResponse>> readAll() {
        System.out.println("READ ALL");
        return null;
    }

    @GetMapping("{id}")
    public ResponseEntity<TodoResponse> readOne() {
        System.out.println("READ ONE");
        return null;
    }

    @PatchMapping("{id}")
    public ResponseEntity<TodoResponse> update() {
        System.out.println("UPDATE");
        return null;
    }

    @DeleteMapping("{id}")
    public ResponseEntity<?> deleteOne() {
        System.out.println("DELETE");
        return null;
    }

    @DeleteMapping
    public ResponseEntity<?> deleteAll() {
        System.out.println("DELETE ALL");
        return null;
    }
}
```
    
</details>
    
## 톰캣 서버 실행
다음과  main() 메서드를 작성 후 run하면 톰캣 서버가 시작했다는 메시지가 뜬다.
- TodoServerApplication
```
package org.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TodoServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(TodoServerApplication.class, args);
    }
}
```
![image](https://user-images.githubusercontent.com/92259017/149104146-9ff28dcd-aa8a-470f-973a-9c0a86168cc7.png)

## Postman
- API는 Postman 앱을 이용해서 테스트 할 것이다.<br>
[Download Postman](https://www.postman.com/downloads/)에서 운영체제에 맞는 버전을 설치하고, 계정을 생성한다.<br>
Postman이 실행되면 다음과 같이 POST 방식으로 localhost:8080 url에 요청을 보낸다.<br>
![image](https://user-images.githubusercontent.com/92259017/149105699-b47f7688-0da2-459e-866a-8061fe1d2c65.png)

- Postman에는 아무것도 뜨지 않지만 intellij를 확인해보면 API가 정상적으로 동작하는 것을 확인할 수 있다.<br>
![image](https://user-images.githubusercontent.com/92259017/149107833-874ebc59-443e-4106-ba43-833228a3a79f.png)

# 2) 컨트롤러 구현 후 Todo 테스트
## 컨트롤러 구현
API가 정상적으로 작동하는 것을 확인했으니 컨트롤러를 마저 구현한다.
<details>
    <summary>코드 보기</summary>
    
```
package org.example.controller;

import lombok.AllArgsConstructor;
import org.example.model.TodoEntity;
import org.example.model.TodoRequest;
import org.example.model.TodoResponse;
import org.example.service.TodoService;
import org.springframework.http.ResponseEntity;
import org.springframework.util.ObjectUtils;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.stream.Collectors;

@CrossOrigin
@RestController
@AllArgsConstructor
@RequestMapping("/")
public class TodoController {

    private final TodoService service;

    @PostMapping
    public ResponseEntity<TodoResponse> create(@RequestBody TodoRequest request) {
        System.out.println("CREATE");

        if (ObjectUtils.isEmpty(request.getTitle()))
            return ResponseEntity.badRequest().build();
        if (ObjectUtils.isEmpty(request.getOrder()))
            request.setOrder(0L);
        if (ObjectUtils.isEmpty(request.getCompleted()))
            request.setCompleted(false);

        TodoEntity result = this.service.add(request);
        return ResponseEntity.ok(new TodoResponse(result));
    }

    @GetMapping
    public ResponseEntity<List<TodoResponse>> readAll() {
        System.out.println("READ ALL");
        List<TodoEntity> list = this.service.searchAll();
        List<TodoResponse> response = list.stream().map(TodoResponse::new)
                .collect(Collectors.toList());
        return ResponseEntity.ok(response);
    }

    @GetMapping("{id}")
    public ResponseEntity<TodoResponse> readOne(@PathVariable Long id) {
        System.out.println("READ ONE");
        TodoEntity result = this.service.searchById(id);
        return ResponseEntity.ok(new TodoResponse(result));
    }

    @PatchMapping("{id}")
    public ResponseEntity<TodoResponse> update(@PathVariable Long id, @RequestBody TodoRequest request) {
        System.out.println("UPDATE");
        TodoEntity result = this.service.updateById(id, request);
        return ResponseEntity.ok(new TodoResponse(result));
    }

    @DeleteMapping("{id}")
    public ResponseEntity<?> deleteOne(@PathVariable Long id) {
        System.out.println("DELETE");
        this.service.deleteById(id);
        return ResponseEntity.ok().build();
    }

    @DeleteMapping
    public ResponseEntity<?> deleteAll() {
        System.out.println("DELETE ALL");
        this.service.deleteAll();
        return ResponseEntity.ok().build();
    }
}
```
    
</details>

## API 테스트
- 이제 다시 Postman에서 Body > raw > JSON 선택 후,
todo 아이템 내용을 입력하고 테스트 요청을 보내면
다음과 같이 1번 id로 아이템이 추가된 것을 확인할 수 있다.<br>
![image](https://user-images.githubusercontent.com/92259017/149119308-97e83859-d30c-4da0-bcfd-865629ec829b.png)

- 마찬가지로 전체 목록 조회, 수정, 삭제 요청 모두 잘 작동하는 것을 확인하였다.<br>
![image](https://user-images.githubusercontent.com/92259017/149119727-7360632d-26ed-42ae-ac61-43d9f8dd0972.png)
![image](https://user-images.githubusercontent.com/92259017/149120107-12ae4ea1-47ff-4bca-ab6c-56a64db43f51.png)
![image](https://user-images.githubusercontent.com/92259017/149120423-0012efca-7544-463d-9f7e-b78c174c65e8.png)


## Todo run test
- [Todo-Backend run test](https://www.todobackend.com/specs/index.html)에서 To do list 테스트를 통과하는 지 확인한다.
![image](https://user-images.githubusercontent.com/92259017/149120834-9fc516fa-6100-450a-85c9-96b4b871cc79.png)
![image](https://user-images.githubusercontent.com/92259017/149120993-830aad5c-cac3-484f-8775-6a1b65a0b225.png)

- [Todo-Backend client](https://www.todobackend.com/client/index.html)에서 url 입력 후 go를 클릭하면 프론트가 구현된 To do list 페이지가 나온다.
![image](https://user-images.githubusercontent.com/92259017/149121678-f13cf651-1c5d-4d06-b42f-b00843f896b5.png)

- 다음과 같이 아이템 추가, 수정, 완료 여부를 테스트 해보았고, 잘 동작하는 것을 확인하였다.
![image](https://user-images.githubusercontent.com/92259017/149122300-d470392c-dfc1-4feb-81cf-7c0e3308af73.png)
![image](https://user-images.githubusercontent.com/92259017/149122359-67f10580-e3af-4b42-b430-56587131f251.png)

