# 서비스 코드 구현

service 패키지에 TodoService 클래스 파일을 생성한다.<br><br>
![image](https://user-images.githubusercontent.com/92259017/149085998-6ef73cc3-8048-42ac-b28d-e0a7cec07c9d.png)

## TodoService
다음의 6가지 기능을 구현하기 위해 서비스 코드에 메서드 시그니처들을 정의한다.
1.	todo 리스트 목록에 아이템을 추가
2.	todo 리스트 전체 목록을 조회
3.	todo 리스트 목록 중 특정 아이템을 조회
4.	todo 리스트 목록 중 특정 아이템을 수정
5.	todo 리스트 목록 중 특정 아이템을 삭제
6.	todo 리스트 전체 목록을 삭제

```
package org.example.service;

import lombok.AllArgsConstructor;
import org.example.model.TodoEntity;
import org.example.model.TodoRequest;
import org.example.repository.TodoRepository;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Service;
import org.springframework.web.server.ResponseStatusException;

import java.util.List;

@Service
@AllArgsConstructor
public class TodoService {

    private final TodoRepository todoRepository;

    public TodoEntity add(TodoRequest request) {
        TodoEntity todoEntity = new TodoEntity();
        todoEntity.setTitle(request.getTitle());
        todoEntity.setOrder(request.getOrder());
        todoEntity.setCompleted(request.getCompleted());
        return this.todoRepository.save(todoEntity);
    }

    public List<TodoEntity> searchAll() {
        return this.todoRepository.findAll();
    }

    public TodoEntity searchById(Long id) {
        return this.todoRepository.findById(id)
                .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
    }

    public TodoEntity updateById(Long id, TodoRequest request) {
        TodoEntity todoEntity = this.searchById(id);
        if (request.getTitle() != null) {
            todoEntity.setTitle(request.getTitle());
        }
        if (request.getOrder() != null) {
            todoEntity.setOrder(request.getOrder());
        }
        if (request.getCompleted() != null) {
            todoEntity.setCompleted(request.getCompleted());
        }
        return this.todoRepository.save(todoEntity);
    }

    public void deleteById(Long id) {
        this.todoRepository.deleteById(id);
    }

    public void deleteAll() {
        this.todoRepository.deleteAll();
    }
}
```
