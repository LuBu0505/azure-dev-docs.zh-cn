---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: d4b9becdce2b78e928b97b7d980024eac5871df2
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369837"
---
在 `DemoApplication` 类之后创建一个新的 `Todo` Java 类：

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

此类是映射在之前创建的 `todo` 表上的域模型。

若要管理该类，你需要一个存储库。 在同一包中定义一个新的 `TodoRepository` 接口：

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

此存储库是 Spring Data R2DBC 管理的响应式存储库。

创建可存储和检索数据的控制器，完成该应用程序。 在同一包中实现 `TodoController` 类，并添加以下代码：

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最后，暂停应用程序并再次启动它：

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>测试应用程序

若要测试应用程序，可使用 cURL。

首先，在数据库中创建一个新的 todo 项：

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

此命令应返回创建的项：

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

接下来，使用新的 cURL 请求检索数据：

```bash
curl http://127.0.0.1:8080
```

此命令会返回“todo”项列表，其中包括已创建的项：

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```
