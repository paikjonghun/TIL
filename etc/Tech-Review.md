# 기술면접 준비

## Java

- 자바의 특징
    1. 자바는 **운영체제에 독립적**으로 실행할 수 있습니다.
    2. 자바는 **자동 메모리 관리** 등을 지원하여 다른 언어에 비해 안정성이 높습니다
    3. 자바에 관한 **수많은 참고 자료**를 찾을 수 있습니다.
    4. **객체지향언어**
    5. **멀티스레드**

- 객체지향개념의 특징
    - OOP(객체 지향 프로그래밍)이란?
        - 데이터를 객체로 취급해서 프로그램에 반영. 순차적으로 동작하는 것이 아니라 객체와 객체의 상호작용으로 프로그램이 동작하는 것
    - 캡슐화
        - 캡슐화란 캡슐처럼 묶어 내부의 구조를 감추는 것을 말한다. 외부에서는 내부의 구조를 알지 못하며, 객체가 노출하여 제공하는 필드와 메소드만 이용할 수 있다.
    - 상속
        - 일반적으로 상속은 부모가 가지고 있는 재산을 물려주는 것을 뜻한다.(메소드, 변수) 하위객체는 상위객체를 재사용하여 쉽고 빠르게 설계할 수 있어, 반복된 코드의 중복을 줄여준다. (개발의 효율성 ↑, 개발 소요시간↓)
    - 다형성
        - 다형성은 하나의 타입에 여러 객체를 대입하여 다양한 기능을 이용할 수 있도록 하는 것을 말한다. 부모 타입에는 모든 자식 객체가 대입될 수 있으며, 인터페이스 타입에는 모든 구현 객체가 대입될 수 있다.
    
- 객체지향개념 장점
    - 코드 재사용성 높음
    - 코드 변경 용이
    - 상속을 통한 장점 극대화

- 자바 8의 특징
    - 람다 표현식(lambda expression-함수형 프로그래밍) 도입
    
- 자바 11의 특징
    
    

- 추상화란?

- AOP란?

- 오버로딩과 오버라이딩
    - Overloading : 같은 이름의 메소드를 여러개 정의하는 것. 중복 정의. 매개변수의 타입이 다르거나 개수가 달라야 한다.
    - Overriding : 상속에서 나온 개념. 상위 클래스(부모 클래스)의 메소드를 하위 클래스(자식 클래스)에서 재정의

- 쿠키란?
    - 사용자 정보를 유지할 수 없다는 HTTP의 한계를 극복할 수 있는 방법

- 세션이란?

- 객체지향(상속, 다형성, 캡슐화 등)
- JVM 메모리 구조
- 컴파일 과정
    - 컴파일러가 소스코드를 자바 바이트 코드(.class)로 변환하고, JVM이 바이트코드를 기계어로 변환하고, 인터프리터 방식으로 애플리케이션 실행
- 다양한 GC(parellel, g1gc 등)
- JRE, JDK, JVM의 구분
- 자바 메모리관리(Xms, Xmx)Call by Value vs Call by ReferenceString Immutable(String constant pool, "a" vs new String("a"))Auto Boxing & UnBoxingError vs ExceptionChecked vs UnChecked Exception
- 비동기처리 문법 비교
- Java 8의 특징
- Lambda(+ Functional Interface)Stream API
- Default MethodGenericReflection(Annotation)Collection Framework(List, Map, Set 등)
- HashMap([https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311))Abstract Class vs Interface(default method)CountDownLatch & CyclicBarrier

## Spring Framework

- DI
- IoC
- 서블릿/JSP
- PSA
- IoC
- AOP
- POJO(각각에 대한 내용과 도입 이유)
- Bean(Scope)
- @Autowired 주입 방법별 차이(Field, Setter, Constructor Injection)
- ApplicationContextWeb
- MVC 요청 처리 과정(DispatcherServlet을 중심으로)
- Spring vs Spring Boot
- @SpringBootApplication의 내부 구성
- @Controller vs @RestController

## DB 관련

- ERD 뭘로 그렸는지?
- 짜는 과정에서 어려웠던 점?
- query 작성 얼마나 해봤는지?
- 어느 단계 정도의 query 를 작성해봤는지?
- ORM에 대한 설명?
- Join 개념?
- 시퀀스란?
- 인덱스(내부 구현, 장단점)
- 정규화
- 트랜잭션(ACID)
- 트랜잭션 격리수준(Read Uncommitted ~ Serializable)
- 테이블 설계(쿼리, 다대다 관계 등)
- RDBMS vs NoSQL
- 대표적인 NoSQL
- JDBC
    - JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다

## REST API

- HTTP request 구성,
- response code들 설명
- 500대 코드의 에러를 처리 해봤는지?
- Token 말고 다른 방식 인증?
- Session?

## HTTP

## 자료구조

- Array
- ArrayList
- LinkedList
- Stack
- Queue
- Heap
- Tree(B Tree, Trie)
- Hash(Hash collision 해결법)

## 네트워크

- OSI 7계층(의미)TCP: Handshake, 흐름제어, 혼잡제어UDP
- http(메서드, 상태코드 등)
- http vs httpsDNS게이트웨이프록시CORS세션, 쿠키JWT(Json Web Token)RESTful API란?

## **운영체제**

- 프로세스(PCB)
- 스레드
- Context switching
- Interrupt
- CPU 스케줄링 / 스케줄러
- DeadLock
- Race Condition(세마포어와 뮤텍스)
- 메모리 단편화
- 가상 메모리
- 요구 페이징