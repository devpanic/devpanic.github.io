---
title: DTO with Spring
date: 2023-12-08 11:33:00 +0800
# date: 2023-12-08
categories: [Spring]
tags: [Java, Spring]
---

### **DTO(Data Transfer Object)란?**

온전히 계층간의 데이터 전송만을 위한 객체를 의미<br>
비즈니스 로직을 제외한 아래의 요소들만 포함한다.<br>
Setter를 포함하지 않을 경우, 불변 객체로 생성할 수 있다.

- 데이터
- Getter / Setter

### **왜 DTO를 사용해야할까?**

1. Entity 객체 내 데이터의 부분적 사용<br>
   API마다 전달해야 할 데이터는 상이하다.<br>
   클라이언트에게 노출되는 데이터 중 민감 정보를 보호해야 한다.<br>
   DTO를 사용함으로써 여러 비즈니스 로직에 유연하게 대처 가능하게 되고, 원하는 데이터만 노출시킬 수 있게 된다.

2. Entity 객체 캡슐화<br>
   데이터 전송 과정에 Entity 객체를 사용하게 되면 데이터가 변질될 수 있다.<br>
   DTO를 사용하지 않으면 요구사항 변경 시에 잦은 Entity 객체 구성의 수정이 필요하며, Entity와 연결된 DB 구성도 수정되어야할 수 있다.

3. Controller ↔ Service 의존도<br>
   Service가 Controller의 파라미터 입력에 의존해 동작하게 되면 의존도가 높아진다.<br>
   DTO를 사용함으로써 두 계층간의 의존도를 낮춰줄 수 있다.

### **Spring에서 DTO 사용하기**

**DTO 클래스 생성**

```java
@Data
public class UserDto {

    private String name;
    private int age;
    private String email;
}
```

`@Data` 애너테이션을 적용하면 아래의 애너테이션을 자동으로 적용할 수 있다.

- `@ToString` : 객체의 데이터를 모두 문자열로 만들어 반환
- `@EqualsAndHashCode`
- `@Getter` / `@Setter`

**Controller에서 DTO 반환하기**

```java
@RestController
public class UserController {

    @GetMapping("/users")
    public List<UserDto> getUserList() {
        // ...
        return userRepository.findAll().stream()
                .map(user -> new UserDto(user.getName(), user.getAge(), user.getEmail()))
                .collect(Collectors.toList());
    }
}
```

- `@RestController` 애너테이션으로 Controller 생성
- `/users` 경로에 GET 요청을 처리하는 메소드를 지정
- 앞서 생성한 **UserDto** 객체를 이용해 모든 사용자를 조회하여 반환하는 메소드를 구현

**Service에서 DTO 사용하기**

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public UserDto getUser(Long id) {
        // ...
        return new UserDto(user.getName(), user.getAge(), user.getEmail());
    }
}
```

- `@Service` 애너테이션으로 Service 생성
- ID로 사용자를 조회하여 DTO로 반환하는 메소드 생성
