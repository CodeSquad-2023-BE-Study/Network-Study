# HTTP 의 조건부 헤더와 검증 요청

### HTTP의 캐시

HTTP 프로토콜은 `last-modified` 헤더와 `etag` 헤더를 통해 캐싱기능을 지원합니다. 주로 `Cache-Control` 헤더와 함께 사용해 캐시 데이터의 변경 여부를 확인합니다.

### Last-Modified

HTTP 에서 제공하는 `Last-Modified` 헤더는 서버의 리소스가 언제 마지막으로 변경되었는지 나타내는 헤더입니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/570e80e4-b709-45d5-85dd-229a4685c572)

브라우저는 서버가 내려준 리소스의 만료를 확인하면 서버에게 `If-modified-since` 헤더를 추가해서 HTTP 요청을 보내게 됩니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/92d0f82a-75b7-4c76-9f35-d37601948425)

이를 받은 서버는 `If-modified-since` 헤더를 통해 리소스의 변경여부를 확인합니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/38a15454-cd6e-45d2-96ca-ce19cb14742c)

만약 리소스가 변경되지 않았다면 `304 NOT MODIFIED` 응답을 내려줍니다. 이때 이 응답코드는 응답 바디를 포함하지 않는 것이 특징입니다.

리소스가 변경되었다면 `200 OK` 응답을 내려줍니다. 이때 응답 바디에는 변경된 리소스 정보가 들어있게 됩니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/347311be-030a-4979-bb02-4c1acb8c46f1)

### ETag

![image](https://github.com/todolist-team2/todo-max/assets/66981851/734d5f9d-4f22-4e8d-a047-898fd82f4b14)

HTTP에서 제공하는 `ETag` 헤더는 캐시용 리소스에 고유한 버전 번호를 달아줄 수 있습니다. 데이터가 변경되었을 경우 이 `ETag` 값을 변경해 다시 클라이언트에게 응답하는 방식으로 동작합니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/14973c42-fb74-4124-bfe9-ad4b993409ce)

클라이언트는 서버로부터 받은 리소스의 캐시 기간이 만료되었을 경우 `If-None-Match` 헤더를 통해 이전에 서버로부터 받은 `ETag` 값을 전송합니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/84f75d6b-d6d3-44bb-8ec6-83e7659fb1ba)

마찬가지로 만약 리소스가 변경되지 않았다면 `304 NOT MODIFIED` 응답을 내려줍니다. 이때 이 응답코드는 응답 바디를 포함하지 않는 것이 특징입니다.

![image](https://github.com/todolist-team2/todo-max/assets/66981851/ce169c50-9cf0-44c9-b3cd-24c79e72d55f)

리소스가 변경되었다면 `200 OK` 응답을 내려줍니다. 이때 응답 바디에는 변경된 리소스 정보가 들어있게 됩니다.

### Last-Modified VS ETag

`Last-Modified` 헤더는 날짜 기반의 캐시 데이터를 확인하는 로직을 사용합니다. 그래서 데이터에 의미없는 코드를 제거하거나 주석을 달았을 경우에도 데이터가 변경됐다고 인지되는 경우가 발생할 수 있습니다.

또한 서버에서 캐시 로직을 관리할 수 없게 됩니다.

반면 `ETag` 헤더는 캐시 데이터에 고유한 버전 정보를 달 수 있기 때문에 서버에서 캐시 로직을 관리할 수 있습니다.

# 결론

HTTP의 캐싱 기능은 `last-modified` 헤더와 `etag` 헤더를 사용합니다. `Last-Modified` 헤더는 리소스가 마지막으로 변경된 시간을 확인하고, `ETag` 헤더는 리소스에 고유한 버전 번호를 달아 캐시 데이터를 관리합니다. `Last-Modified` 헤더는 주석이나 의미없는 코드를 제거해도 데이터가 변경됐다고 인지할 수 있고, `ETag` 헤더는 서버에서 캐시 로직을 관리할 수 있습니다.

또한 서버의 리소스가 변경되지 않았을 경우 응답 바디가 없는 `304 NOT MODIFIED` 응답을 주기 때문에 네트워크의 부하를 줄일 수 있습니다.
