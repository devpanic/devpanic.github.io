---
title: application.yml vs application.properties
date: 2024-01-21 12:00:00 +0800
categories: [Spring]
tags: [Spring, Spring Boot, IntelliJ]
---

이전에는 책을 보면서 java project내에서 차근차근 Spring boot 환경을 구성해 나갔었는데, 이 과정을 생략하고 프로젝트를 만들었더니 비어있는 [application.properties](http://application.properties) 파일이 생성되어 있었다.

기존에는 application.yml 파일을 사용했던 터라 두 파일 형식의 차이가 궁금해서 찾아보게 되었다.

<img src="/assets/img/240121/application_properties.png" width="70%" height="70%" title="application.properties" align="left">

### **파일의 용도**

외부 모듈(ex, Database)을 사용할 때 관련된 환경 변수 설정을 지원하여 Spring Application과 연동할 수 있게 한다.<br>
IntelliJ의 경우 Spring Boot 프로젝트 생성 시 application.properties를 지원한다.<br>
두 파일을 함께 사용할 경우 `.properties` 파일이 `.yml` 파일보다 더 높은 우선 순위를 가진다.

### **application.properties**

properties 파일은 환경 변수와 설정 값을 `key=value(문자열)`로 매칭한다. <br>
또한 properties 파일은 Java에서만 사용하며 다수의 Profile을 하나의 파일 안에서 구분할 수 있다. <br>
이때 구분자로는 `#---`을 사용한다.

```java
spring.jpa.show-sql="true"
spring.jpa.properties.hibernate.format_sql="true"
spring.jpa.properties.defer-datasource-initialization="true"
spring.datasource.url="jdbc:h2:mem:testdb"
spring.h2.console.enabled="true"
```

### **application.yml**

yml 파일은 계층적 구조를 조금 더 가독성있게 표현한다. <br>
properties 파일은 계층적 구조를 들여쓰기 구분 없이 표현했지만 yml 파일은 들여쓰기를 이용한 표현으로 더 직관적이다.

```java
spring:
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true
    defer-datasource-initialization: true
  datasource:
    url: jdbc:h2:mem:testdb
  h2:
    console:
      enabled: true
```