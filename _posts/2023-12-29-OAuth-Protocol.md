---
title: OAuth Protocol
date: 2023-12-29 11:33:00 +0800
# date: 2023-12-08
categories: [Protocol]
tags: [Protocol, Authentication, Authorization, HTTP, OAuth]
---

### **OAuth Protocol ?**

구글, 페이스북, 카카오, 네이버 등 다양한 플랫폼에 저장되어 있는 리소스를 이용하기 위한 권한 위임 과정의 표준 프로토콜이다.

### **왜 OAuth Protocol을 사용하게 되었을까 ?**

사용자는 여러개의 웹 서비스를 사용하면서 서비스간 연동이 필요한 경우가 생긴다. <br>
이를 서비스 차원에서 지원하기 위해 타 서비스의 리소스를 이용하는 방식을 사용하곤 했는데 로그인 방식에서 몇가지 문제점이 있었다. <br>

1. 사용자가 타 서비스의 리소스를 요구하는 요청 발생
2. 타 서비스 로그인을 위한 아이디-비밀번호 입력 요구
3. 타 서비스의 로그인 정보를 기반으로 현재 서비스에서 필요한 기능 수행

이러한 방식은 로그인 정보 전달 과정에서 **보안에 취약**하며 어플리케이션 별로 인증 방식이 다르기 때문에 **구현 과정에서 비용이 증가**했다. <br>
문제점을 개선하고자 인증 방식을 표준화한 OAuth Protocol이 발표되었고 현재 대부분의 웹 어플리케이션은 OAuth를 기반으로 동작하고 있다.

### **OAuth2.0 구성 요소**

**Resource Owner** <br>
사용자의 리소스 요청을 받는 주체

**Client** <br>
타 서비스의 리소스를 요청하는 주체. Token을 이용해 Resorce Server에서 필요한 리소스를 제공받는다.

**Resource Server** <br>
요청받은 리소스를 관리하는 주체. 권한이 있는 사용자에게만 접근을 허용한다.

**Authorization Server** <br>
리소스에 대한 접근을 제어하는 주체. 인가에 성공하면 Client에게 Access Token을 발급한다.

### **OAuth의 동작 방식**

<img src="/assets/img/231229/OAuthFlow.png" width="70%" height="70%" title="OAuth Flow"/>

**A.** Client는 Resource Owner에게 인가를 요청한다. 혹은 Authorization Server로 직접 인가를 요청할 수도 있다.

**B.** Client는 권한 위임 방법에 대한 정보가 담긴 권한 획득 자격(Authorization Grant)을 Resource Owner로부터 전달받는다.

**C.** 전달받은 인가를 이용해 Authorization Server로 Access Token을 요청한다.

**D.** Authorization Server는 Client를 인증한 뒤에 요청의 유효성을 검증하고 Access Token을 발급한다.

**E.** 발급받은 Token을 이용해 Resource Server에 리소스를 요청한다.

**F.** Resource Server는 Token의 유효성을 검증하고 요청에 대한 리소스를 전달한다.

### **Ref**

https://datatracker.ietf.org/doc/html/rfc6749
