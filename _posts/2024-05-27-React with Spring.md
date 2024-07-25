---
title: React와 Spring로 프로젝트 구성하기
date: 2024-05-27 12:00:00 +0800
categories: [Spring]
tags: [Java, Spring, React]
---

### React와 Spring을 하나의 레포지토리 내에 구성하기

리액트와 함께 구현하기 위해 프로젝트를 구성했는데 아래와 같은 에러를 확인했다.

```java
You are running `create-react-app` 5.0.0, which is behind the latest release (5.0.1).
```

에러로그를 따라서 아래와 같이 커맨드를 입력하니 손쉽게 해결할 수 있었다.

```java
$ npm uninstall -g create-react-app
$ npm i create-react-app
$ npx create-react-app ${front-app-name}
```

이후 아래의 포스트들을 참고하며 리액트와 스프링 연결 테스트를 진행했다. <br>
[https://velog.io/@u-nij/Spring-Boot-React.js-개발환경-세팅](https://velog.io/@u-nij/Spring-Boot-React.js-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EC%84%B8%ED%8C%85)<br>
[https://velog.io/@ung6860/React-Spring-프로젝트-생성](https://velog.io/@ung6860/React-Spring-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1)<br><br>

Proxy 서버를 설정하고 같은 포트를 사용하도록 구성했는데도 의도한대로 동작하지 않는 문제가 있었다.<br>
차이점이 뭘지 고민을 해봤는데, 나는 controller 패키지를 새로 만들고 그 안에 테스트를 위한 컨트롤러를 만들어 주었다.<br>
이로 인해서 SpringApplication.run() 메소드가 실행되는 패키지와 다른 패키지에 위치하게 되었고 내가 사용하고자 하는 컨트롤러의 빈이 생성되지 못한 것이 문제가 되었다.<br>
경로를 재조정해준 뒤 다시 빌드했더니 잘 동작하였다.<br>
![application](/assets/img/240527/server.png)<br><br>
동작은 하지만 이렇게 환경을 구성하게 된 데에 CORS 오류가 많은 영향을 끼쳤는데 좀 더 조사가 필요할 것 같다.
