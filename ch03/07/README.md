# 테스트 코드 작성

build.gradle 파일에 테스트에 필요한 라이브러리를 추가하고, Gradle 탭에서 Reload 한다.<br><br>
<img src="https://user-images.githubusercontent.com/92259017/149125957-1f4924a6-d74d-4a15-843a-7aa1dbb9f375.png" style="width: 70%; height:70%">

## TodoService 테스트
### 테스트 생성
TodoService 파일에 와서 우클릭 > Go To > Test 를 클릭하여 테스트를 생성한다.<br><br>
<img src="https://user-images.githubusercontent.com/92259017/149134812-f613d116-3821-401d-9caf-0f3c09022582.png" style="width: 70%; height:70%">

테스트 메서드를 작성할 메서드를 선택한 후 OK 클릭<br><br>
<img src="https://user-images.githubusercontent.com/92259017/149135030-ab453ae2-49d7-4e97-8a6d-5436e5193d29.png" style="width: 50%; height:50%">

### TodoServiceTest
- Mock을 사용하는 이유
1. 외부 시스템(네트워크, DB 연결 상태)에 의존하지 않고 자체 테스트를 수행하기 위해
2. 테스트 할 때마다 DB에 값이 추가, 수정, 삭제되면 문제가 생길 수 있기 때문에

테스트 코드 작성 후 실행하여 성공적으로 빌드되는지 확인한다.
```
package org.example.service;

import org.example.model.TodoEntity;
import org.example.model.TodoRequest;
import org.example.repository.TodoRepository;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.AdditionalAnswers;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.web.server.ResponseStatusException;

import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.ArgumentMatchers.anyLong;
import static org.mockito.BDDMockito.given;
import static org.mockito.Mockito.when;

@ExtendWith(MockitoExtension.class)
class TodoServiceTest {

    @Mock
    private TodoRepository todoRepository;

    @InjectMocks
    private TodoService todoService;

    @Test
    void add() {
        when(this.todoRepository.save(any(TodoEntity.class)))
                .then(AdditionalAnswers.returnsFirstArg());

        TodoRequest expected = new TodoRequest();
        expected.setTitle("Test Title");

        TodoEntity actual = this.todoService.add(expected);

        assertEquals(expected.getTitle(), actual.getTitle());
    }

    @Test
    void searchById() {
        TodoEntity entity = new TodoEntity();
        entity.setId(123L);
        entity.setTitle("TITLE");
        entity.setOrder(0L);
        entity.setCompleted(false);

        Optional<TodoEntity> optional = Optional.of(entity);
        given(this.todoRepository.findById(anyLong()))
                .willReturn(optional);

        TodoEntity expected = optional.get();

        TodoEntity actual = this.todoService.searchById(123L);

        assertEquals(expected.getId(), actual.getId());
        assertEquals(expected.getTitle(), actual.getTitle());
        assertEquals(expected.getOrder(), actual.getOrder());
        assertEquals(expected.getCompleted(), actual.getCompleted());
    }

    @Test
    public void searchByIdFailed() {
        given(this.todoRepository.findById(anyLong()))
                .willReturn(Optional.empty());
        assertThrows(ResponseStatusException.class, () -> {
            this.todoService.searchById(123L);
        });
    }
}
```

## TodoController 테스트
```
package org.example.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.example.model.TodoEntity;
import org.example.model.TodoRequest;
import org.example.service.TodoService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.web.server.ResponseStatusException;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.BDDMockito.given;
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.delete;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(TodoController.class)
class TodoControllerTest {

    @Autowired
    MockMvc mvc;

    @MockBean
    TodoService todoService;

    private TodoEntity expected;

    @BeforeEach
    void setup() {
        this.expected = new TodoEntity();
        this.expected.setId(123L);
        this.expected.setTitle("TEST TITLE");
        this.expected.setOrder(0L);
        this.expected.setCompleted(false);
    }

    @Test
    void create() throws Exception {
        when(this.todoService.add(any(TodoRequest.class)))
                .then((i) -> {
                    TodoRequest request = i.getArgument(0, TodoRequest.class);
                    return new TodoEntity(this.expected.getId(), request.getTitle()
                            , this.expected.getOrder(), this.expected.getCompleted());
                });

        TodoRequest request = new TodoRequest();
        request.setTitle("ANY TITLE");

        ObjectMapper mapper = new ObjectMapper();
        String content = mapper.writeValueAsString(request);

        this.mvc.perform(post("/")
                    .contentType(MediaType.APPLICATION_JSON)
                    .content(content))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.title").value("ANY TITLE"));
    }

    @Test
    void readOne() throws Exception {
        given(todoService.searchById(123L)).willReturn(expected);

        mvc.perform(get("/123"))
                .andExpect(status().isOk())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$.id").value(expected.getId()))
                .andExpect(jsonPath("$.title").value(expected.getTitle()))
                .andExpect(jsonPath("$.order").value(expected.getOrder()))
                .andExpect(jsonPath("$.completed").value(expected.getCompleted()));
    }

    @Test
    void readOneException() throws Exception {
        given(todoService.searchById(123L)).willThrow(new ResponseStatusException(HttpStatus.NOT_FOUND));

        mvc.perform(get("/123"))
                .andExpect(status().isNotFound());
    }

    @Test
    void readAll() throws Exception {
        List<TodoEntity> mockList = new ArrayList<>();
        int expectedLength = 10;
        for (int i = 0; i < expectedLength; i++) {
            mockList.add(mock(TodoEntity.class));
        }

        given(todoService.searchAll()).willReturn(mockList);

        mvc.perform(get("/"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.length()").value(expectedLength));
    }

    @Test
    void deleteAll() throws Exception {
        mvc.perform(delete("/"))
                .andExpect(status().isOk());
    }
}
```

## 참고자료
- [IntelliJ IDEA에 JUnit 추가하기 / 테스트 코드 작성 by 일단](https://ildann.tistory.com/5)
