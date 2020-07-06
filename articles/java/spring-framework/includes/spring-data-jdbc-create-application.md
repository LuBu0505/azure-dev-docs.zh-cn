---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 012043df3cf07de098d1a7f3a6715374814d1d9b
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507688"
---
在 `DemoApplication` 类旁创建新的 `Todo` Java 类并添加以下代码：

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

import org.springframework.data.repository.CrudRepository;

public interface TodoRepository extends CrudRepository<Todo, Long> {
}
```

此存储库是 Spring Data JDBC 管理的存储库。

创建可存储和检索数据的控制器，完成该应用程序。 在同一包中实现 `TodoController` 类，并添加以下代码：

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Todo createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Iterable<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最后，使用以下命令暂停应用程序并再次启动它：

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>测试应用程序

若要测试应用程序，可使用 cURL。

首先，使用以下命令在数据库中创建一个新的待办事项：

```bash
curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done": "true"}' \
    http://127.0.0.1:8080
```

此命令应返回创建的项，如下所示：

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done":true}
```

接下来，使用新的 cURL 请求检索数据，如下所示：

```bash
curl http://127.0.0.1:8080
```

此命令会返回待办事项列表，其中包括已创建的项，如下所示：

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done":true}]
```
