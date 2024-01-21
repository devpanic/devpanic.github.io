---
title: JVM 알아보기 - 1. JVM? / Class Loader
date: 2023-12-31 11:33:00 +0800
categories: [Java]
tags: [Java, JVM]
---

### **0. JVM이란?**

Java 어플리케이션은 실행에 있어서 운영체제에 종속적이지 않기 위해 Java 가상 머신을 제공한다. <br>
이를 통해 컴파일된 운영체제로의 종속 없이 어플리케이션을 실행 가능하며, Kotlin과 Scala에서도 동일 모델을 사용한다.

### **1. JVM의 구성 요소**
1. Java Class Loader : 클래스 파일 실행을 위한 요소 제공
   - Bootstrap Class Loader
   - Extension Class Loader
   - Application Class Loader
2. Execution Engine : 바이트 코드의 기계어 변환
3. Runtime Data Area : 실행 데이터 저장을 위한 공간

### **2. Java Class Loader**
Java Class Loader는 클래스 파일을 Loading, Linking, Initialization 과정을 통해 어플리케이션 실행 전체 주기에 관여한다. <br>

**Loading**<br>
클래스 파일을 읽고 바이너리 데이터를 생성해 객체를 JVM Heap 메모리에 Load한다.<br>
ClassLoader 클래스의 loadClass 메소드를 통해 이루어지며 비용이 큰 작업이기에 옵션으로 설정할 수 있다.<br>

**Linking**<br>
로드된 클래스 파일에 대해서 Verification, Preparation, Resolution의 과정을 수행한다.<br>
1. Verification : 바이트 코드가 수정되었는지 클래스 파일의 유효성을 판단한다.
2. Preparation : 클래스 변수와 메소드 테이블 초기화를 위한 메모리 공간을 할당한다.
3. Resolution : 필수적인 과정은 아니다. Symbolic Reference로 사용하던 부분을 실제 메모리 주소로 교체해주는 작업이다.

**Initialization**<br>
Static 변수의 값을 할당한다. 이외에도 SuperClass의 초기화를 진행한다. Thread-Safety 환경을 보장한다.

### **3. Class Loader의 종류**
* **Bootstrap Class Loader**<br>
최상위 Class Loader로 JVM이 초기화될 때 가장 먼저 생성되며 실행된다. <br>
다른 Class Loader들은 Bootstrap Class Loader 에서 로드된 클래스들을 부모로 갖게된다.<br>
`rt.jar`과 같은 **핵심 클래스**를 Load하는 역할이다.<br>
* **Extension Class Loader**<br>

* **Application Class Loader**<br>


### **3.**

### **Ref**
- https://nesoy.github.io/articles/2020-11/ClassLoader