---
title: MissingServletRequestParameterException 예외 발생 분석
date: 2024-05-25 11:33:00 +0800
categories: [Spring]
tags: [Java, Spring, Postman]
---

MySQL을 사용해서 Spring 프로젝트를 구성하고 싶어서 공식 [문서](https://spring.io/guides/gs/accessing-data-mysql#initial)를 통해서 실습 프로젝트를 생성했다.<br>
MySQL Database 환경 구성을 한 뒤에 Postman을 이용해서 테스트를 진행했다.<br>

API는 다음과 같다.
### 1. POST /demo/add
name, email 데이터를 연결한 DB에 저장한다.

### 2. GET /demo/all
`select * from 연결된DB` 요청을 처리한다.

2번은 잘 동작했지만 1번 항목에서 MissingServletRequestParameter 에러가 발생했다.<br>
이를 해결하기 위해서 먼저 값이 Postman을 통해서 잘 넘어가는지 확인하기 위해서 POST 방식을 GET 방식으로 바꾸고 URL에 Parameter를 추가했다. <br>
Postman을 통해 처리한 GET 요청은 성공적으로 DB에 데이터를 저장하는 것을 관찰했다. <br>
그래서 POST 방식 처리 단계에 문제가 있다고 생각했고 하단의 코드에서 놓친 것이 있는지 확인했다. <br>
```
 @PostMapping(path="/add") // Map ONLY POST Requests
  public @ResponseBody String addNewUser (@RequestParam String name, @RequestParam String email) { ... }
```
---
먼저 @RequestParam을 잘못사용하고 있는지 알아보기 위해서 발생한 예외와 연관지어 검색을 했다. <br>
RequestParam을 통해 넘겨주는 데이터는 required 옵션에 대해 true가 디폴트 값으로 설정되어 있어서 발생하는 것이라는 포스트가 대다수였다. <br>
그런데 나는 논리적으로 name, email 모든 데이터를 required=true인 상태에서 넘겨주고 싶었고 단순히 required=false로 새로 옵션 값을 지정하는 것은 근본적인 해결 방법이 아니라고 생각했다.<br><br>
그래서 추가적인 검색을 통해 원인을 찾아보니 Postman으로 데이터를 넘겨주는 POST 요청을 사용할 경우 @RequestParam 매개변수와 넘겨준 데이터를 매핑하지 못한다는 사실을 확인했다. <br>
해결의 실마리를 찾기 위해 GPT에 방법을 검색했고 @RequestBody 애너테이션을 이용해 DTO에 데이터를 저장해 넘겨주는 방식을 사용하면 된다는 답변을 받았다. <br>
여기서 궁금증이 생겼는데, DTO를 사용하지 않고 아래 방법처럼 여러 데이터를 모두 @RequestBody로 넘겨주는 방식은 안되는 걸까? 싶었다. <br>
```
 @PostMapping(path="/add") // Map ONLY POST Requests
  public @ResponseBody String addNewUser (@RequestBody String name, @RequestBody String email) { ... }
```
이렇게 코드를 재구성해서 확인해보니 컴파일 에러가 발생했고 HttpMessageNotReadableException을 발생시켰다.<br>
아무래도 여러개의 인자 각각에 @RequestBody를 사용했기 때문에 HTTP Packet Body Parsing 에러가 나는 듯 했으나 정확한 부분은 추가적인 공부가 필요하다. <br>

---
GPT에게 제안받은 대로 UserDTO를 구성했고 이를 사용해서 데이터를 저장하도록 코드를 변경했다.
```
// UserDTO.java
@Getter
@Setter
public UserDTO{
  @Id
  String name;
  String email;
}
...
// MainController.java
...
 @PostMapping(path="/add") // Map ONLY POST Requests
  public @ResponseBody String addNewUser (@RequestBody UserDTO userDTO) { ... }
```
코드를 변경하니 더이상의 MissingServletRequestParameterException이 발생하지 않는 것을 알 수 있었다. <br><br>

해결은 했으나 아래와 같은 부분을 추가적으로 스터디해야 할 것 같다.

1. @RequestParam / @RequestBody의 차이점
2. @RequestBody를 이용한 매개변수를 여러개 이용할 경우 HTTPMessageNotReadableException이 발생하는 이유
