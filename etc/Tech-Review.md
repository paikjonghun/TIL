# 기술면접 준비

# Java

- **자바의 특징**
    1. 자바는 **운영체제에 독립적**으로 실행할 수 있습니다.
    2. 자바는 **자동 메모리 관리(GC)** 등을 지원하여 다른 언어에 비해 안정성이 높습니다
    3. **객체지향언어**
    4. **멀티스레드**

- **객체지향개념의 특징**
    - **OOP(객체 지향 프로그래밍)이란?**
        - 데이터를 객체로 취급해서 프로그램에 반영. 순차적으로 동작하는 것이 아니라 **객체와 객체의 상호작용**으로 프로그램이 동작하는 것
    - **객체지향개념 장점**
        - 코드 재사용성 높음
        - 코드 변경 용이
        - 상속을 통한 장점 극대화
    - **캡슐화**
        - **한 객체가 특정한 하나의 목적을 위해 필요한 데이터나 메소드를 하나로 묶는 것을 의미한다**
        - 캡슐화란 캡슐처럼 묶어 내부의 구조를 감추는 것을 말한다. 외부에서는 내부의 구조를 알지 못하며, 객체가 노출하여 제공하는 필드와 메소드만 이용할 수 있다.
        - **은닉화**
            - **캡슐화의 목표. 내부 구조는 private하게 감춰두고 외부에서 조작할 수 있는 정보만 public으로 공개한다**
    - **상속**
        - **기존 메소드와 변수를 물려받되, 필요한 기능을 더 추가하거나 나(자식클래스)에게 맞게 재정의하는 방법**
        - 일반적으로 상속은 부모가 가지고 있는 재산을 물려주는 것을 뜻한다.(메소드, 변수) 하위객체는 상위객체를 재사용하여 쉽고 빠르게 설계할 수 있어, 반복된 코드의 중복을 줄여준다. (개발의 효율성 ↑, 개발 소요시간↓)
    - **다형성**
        - **하나의 변수명이 상황에 따라 다른 의미로 해석될 수 있다는 것을 뜻한다.**
        - 다형성은 하나의 타입에 여러 객체를 대입하여 다양한 기능을 이용할 수 있도록 하는 것을 말한다. 부모 타입에는 모든 자식 객체가 대입될 수 있으며, 인터페이스 타입에는 모든 구현 객체가 대입될 수 있다.
    - **추상화**
        - **공통의 속성이나 기능을 묶어 이름을 붙이는 것이다**
    
- **자바 8의 특징**
    - 람다 표현식(lambda expression-함수형 프로그래밍) 도입
    - 함수형 인터페이스 도입
    
- **자바 11의 특징**
    - javac를 통해 컴파일 하지 않고 바로 자바로 실행 가능하게 됨.
    - 새로운 GC가 나옴.

- **오버로딩과 오버라이딩**
    - Overloading : 같은 이름의 메소드를 여러개 정의하는 것. 중복 정의. 매개변수의 타입이 다르거나 개수가 달라야 한다.
    - Overriding : 상속에서 나온 개념. 상위 클래스(부모 클래스)의 메소드를 하위 클래스(자식 클래스)에서 재정의

- **쿠키란?**
    - 사용자 정보를 유지할 수 없다는 HTTP의 한계를 극복할 수 있는 방법
    - 사용자의 컴퓨터에 저장하는 작은 기록 정보 파일입니다. HTTP에서 클라이언트의 상태 정보를 PC에 저장했다가 필요시 정보를 참조하거나 재사용할 수 있습니다.

- **세션이란?**
    - **세션은** 일정 시간동안 같은 사용자로부터 들어오는 일련의 요구를 하나의 상태로 보고, 그 상태를 유지시키는 기술입니다. 즉, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라고 합니다.

- JVM 역할
    - JVM은 스택 기반으로 동작하며, Java Byte Code를 OS에 맞게 해석 해주는 역할을 하고 가비지컬렉션을 통해 자동적인 메모리 관리를 해줍니다.

- JVM 메모리 구조
    - Class Loader
        - JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈입니다. 런타임 시에 동적으로 클래스를 로드.
    - Execution Engine
        - 클래스 로더를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명렁어 단위로 읽어서 실행합니다.
    - Garbage Collector
        - 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할
    - Runtime Data Area
        - JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역
        - Method Area
        - Heap Area
        - Stack Area
        - PC register
        - Native Method Stack

- 컴파일 과정
    - 컴파일러가 소스코드를 자바 바이트 코드(.class)로 변환하고, JVM이 바이트코드를 기계어로 변환하고, 인터프리터 방식으로 애플리케이션 실행

- 다양한 GC(parellel, g1gc 등)

- JRE, JDK, JVM의 구분
    - JDK - 자바 개발 도구
        - JRE + 개발을 위해 필요한 도구들(java, javac)
    - JRE - 자바 실행 환경
        - JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다. JRE는 JVM의 실행환경을 구현했다고 할 수 있다.
    - JVM - 자바 가상 머신

- 자바 메모리 영역
    - Method 영역
        - 전역변수와 static변수를 저장하며, Method영역은 프로그램의 시작부터 종료까지 메모리에 남아있다.
    - Stack 영역
        - 지역변수와 매개변수 데이터 값이 저장되는 공간이며, 메소드가 호출될 때 메모리에 할당되고 종료되면 메모리가 해제된다. LIFO(Last In First Out) 구조를 갖고 변수에 새로운 데이터가 할당되면 이전 데이터는 지워진다.
    - Heap 영역
        - new 키워드로 생성되는 객체(인스턴스), 배열 등이 Heap 영역에 저장되며, 가비지 컬렉션에 의해 메모리가 관리되어 진다.

- 자바 메모리 영역이 할당되는 시점
    - Method 영역 : JVM이 동작해서 클래스가 로딩될 때
    - Stack 영역 : 런타임 시 할당
    - Heap 영역 : 컴파일 타임 시 할당

- 클래스와 객체
    - 클래스는 객체를 만들어내기 위한 설계도 혹은 틀 이라고 할 수 있고, 객체를 생성하는데 사용합니다. 객체는 설계도(클래스)를 기반으로 생성되며, 자신의 고유 이름과 상태, 행동을 갖습니다.

- 자바 메모리관리(Xms, Xmx)

- **Call by Value vs Call by Reference**
    - Call by Value는 값을 복사하여 처리.
    - Call by Reference는 값을 직접 참조. 원래 값이 영향을 받는다.
    - 메소드 매개변수에 기본 타입 변수를 담는지, 참조 타입 변수를 담는지.

- **static의 위험성**
    - static은 클래스 로딩 시, 메모리 공간을 할당하여, 처음 설정된 메모리 공간이 변하지 않고 프로그램이 종료될 때 해제됩니다. 보통의 경우 여러 동작에서 공통적으로 요구되는 것들을 정적으로 설정하여 사용합니다.

- String Immutable(String constant pool, "a" vs new String("a"))
- Auto Boxing & UnBoxing
- Error vs Exception
- Checked vs UnChecked Exception
- 비동기처리 문법 비교
- Java 8의 특징
- Lambda(+ Functional Interface)
- Stream API
- Default Method
- GenericReflection(Annotation)

- **Collection Framework(List, Map, Set 등)**
    - List - 중복을 허용하고 순서를 가짐
    - Map - key와 value의 한 쌍 형태로 지정
    - Set - 중복을 허용하지 않고 순서를 가지지 않음.

- HashMap([https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311))
- Abstract Class vs Interface(default method)
- CountDownLatch & CyclicBarrier

# **개발 시 중요한 요소**

- 예외 처리
- 테스트
- 협업

# Spring Framework

- API : 응용 프로그램을 작성할 때 필요한 **매개체**
- Framework : 개발을 편리하게 하기 위해서 **미리 만들어놓은 틀.** 틀대로 만들어야 함.
- Library : 도서관. 프로그램 개발을 위해 **가져다 쓰는 소프트웨어**

- DI
    - 의존성 주입은 필요한 객체를 직접 생성하는 것이 아닌 외부로부터 객체를 받아서 사용하는 것입니다.

- **IoC - 제어의 역전**
    - 제어의 역전(IoC)란 모든 객체에 대한(생성, 라이프사이클 등) 제어권을 개발자가 아닌 IoC 컨테이너에게 넘긴 것을 말합니다.
    - 컨트롤의 제어권이 사용자가 아닌 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다.

- 서블릿/JSP
    - 서블릿 - HTML in Java
    - JSP - Java in HTML

- PSA

- **AOP란?**
    - 다양한 부분에서 공통적으로 사용되는 관심사를 단일 기능으로 분리해서 코드의 중복을 줄이고 관리를 편리하게 만드는 개발 방법

- POJO(각각에 대한 내용과 도입 이유)

- Bean(Scope)
    - 빈 등록하는 방법
        - @Component 어노테이션 붙이기(@Controller, @Service, @Repository 는 다 포함하고 있음)

- @Autowired 주입 방법별 차이(Field, Setter, Constructor Injection)

- ApplicationContextWeb

- Spring **MVC란?**
    - Model - 데이터 관리 및 비즈니스 로직을 처리하는 부분.
    - View - 비즈니스 로직의 처리 결과를 통해 유저 인터페이스가 표현되는 구간.
    - Controller - 사용자의 요청을 처리하고 Model과 View를 중개하는 역할.

- **MVC 요청 처리 과정(DispatcherServlet을 중심으로)**
    
    1. 클라이언트는 URL을 통해 요청을 전송한다.
    
    2. 디스패처 서블릿은 핸들러 매핑을 통해 해당 요청이 어느 컨트롤러에게 온 요청인지 찾는다.
    
    3. 디스패처 서블릿은 핸들러 어댑터에게 요청의 전달을 맡긴다.
    
    4. 핸들러 어댑터는 해당 컨트롤러에 요청을 전달한다.
    
    5. 컨트롤러는 비즈니스 로직을 처리한 후에 반환할 뷰의 이름을 반환한다.
    
    6. 디스패처 서블릿은 뷰 리졸버를 통해 반환할 뷰를 찾는다.
    
    7. 디스패처 서블릿은 컨트롤러에서 뷰에 전달할 데이터를 추가한다.
    
    8. 데이터가 추가된 뷰를 반환한다.
    

- **Spring Framework와 Spring Boot 의 차이**
    - 가장 큰 차이점은 Auto Configuration. 내장 톰캣의 차이.

- @SpringBootApplication의 내부 구성

- @Controller vs @RestController

- MVC 모델1 과 모델2의 차이?
    - **View와 Controller의 분리 여부.**
    - 모델1은 하나의 jsp에서 요청도 받고 응답도 하도록 만들어짐.
    - 모델2는 요청을 받는 컨트롤러를 분리한 것 코드 가독성 향상, 유지보수 용이.
    
- **@RequestBody**
    - 클라이언트가 전송하는 JSON 형태의 HTTP Body 내용을 MessageConverter를 통해 Java Object로 변환시켜주는 역할을 합니다.

- **@RequestParam**
    - 1개의 HTTP 요청 파라미터를 받기 위해 사용

- **@ModelAttribute**
    - HTTP Body 내용과 HTTP 파라미터의 값들을 생성자,Getter,Setter를 통해 주입하기 위해 사용

# DB 관련

- ERD 뭘로 그렸는지?
- 짜는 과정에서 어려웠던 점?
- query 작성 얼마나 해봤는지?
- 어느 단계 정도의 query 를 작성해봤는지?
- ORM에 대한 설명?

- **Join 개념?**
    - 두 테이블을 결합하는 연산
    - inner join은 두 테이블에서 서로 연관된 내용만 결합. 교집합
    - outter join은 한 쪽에는 데이터가 있고, 한 쪽에는 데이터가 없는 경우 데이터가 있는 쪽의 내용은 전부 출력하며 결합하는 방법

- 시퀀스란?
- 인덱스(내부 구현, 장단점)
- 정규화
- 트랜잭션(ACID)
- 트랜잭션 격리수준(Read Uncommitted ~ Serializable)
- 테이블 설계(쿼리, 다대다 관계 등)
- RDBMS vs NoSQL

- 대표적인 NoSQL
    - 몽고DB

- **JDBC**
    - JDBC(Java Database Connectivity)는 **자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API**이다

- **MyBatis**
    - 자바의 관계형 DB 프로그래밍을 좀더 수월하게 할수있게 도와준다. JDBC의 작업을 간편하게 해주는 프레임워크이다.
    - 마이바티스는 JDBC로 처리하는 상당부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
    

# REST API

- HTTP request 구성,
- response code들 설명
- 500대 코드의 에러를 처리 해봤는지?
- Token 말고 다른 방식 인증?
- Session?
    - 클라이언트와 웹 서버 상의 네트워크의 연결이 지속적으로 유지되고 있는 상태 (ex.로그인 상태 유지중)
- Cookie?
    - 웹 브라우저에 보관하고 있는 데이터로, "key"와 "value"로 구성 (ex.자동 로그인 기능)

# 자료구조

- Array
- ArrayList
- LinkedList
- **Stack**
    - 후입선출, LIFO 구조
    - 가장 마지막에 삽입된 자료가 가장 먼저 삭제됨

- **Queue**
    - 선입선출, FIFO 구조
    - 가장 먼저 삽입된 자료가 가장 먼저 삭제됨.

- Heap
- Tree(B Tree, Trie)
- Hash(Hash collision 해결법)

# 네트워크

- OSI 7계층(의미)
- TCP: Handshake, 흐름제어, 혼잡제어UDP
- http(메서드, 상태코드 등)란?
- get방식
- post방식
- http vs https
- DNS
- 게이트웨이
- 프록시
- CORS
- 세션, 쿠키
- JWT(Json Web Token)
- RESTful API란?

# **운영체제**

시스템의 자원과 동작을 관리하는 소프트웨어. 프로세스, 저장장치, 네트워킹, 사용자, 하드웨어를 관리

메모리 구조 - 코드, 데이터, 힙, 스택 영역으로 나눠져있다.

- 프로세스(PCB)
    - 실행중인 프로그램

- 스레드
    - 프로세스 안 실행 단위

- Context switching
- Interrupt

- CPU 스케줄링 / 스케줄러
    - 준비큐 안에 있는 프로세스에 대해 CPU를 할당하는 방법

- DeadLock
- Race Condition(세마포어와 뮤텍스)
- 메모리 단편화
- 가상 메모리
- 요구 페이징