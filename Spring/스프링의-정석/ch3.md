### 1. 변경에 유리한 코드(1) - 다형성, factory method

- 변경 사항이 발생했을 때, 변경 포인트를 줄이기 위함. 어떻게 하면 변경에 유리한 코드를 작성할 것인가에 대한 것

```java
// 1. 위 코드를 아래 코드처럼 바꿀 때
SprotsCar car = new SprotsCar();
Truck car = new Truck();

// 2. 이렇게 바꾸는 것이 변경에 유리함.
Car car = new SportsCar();
Car car = new Truck(); 
```

```java
// 3. 이렇게 또 바꾸는 것이 더 변경에 유리함.
Car car = getCar();

static Car getCar() {
	return new SportsCar();
}
```

### 2. 변경에 유리한 코드(2) - Map과 외부 파일

```java
Car car = getCar();
static Car getCar() throws Exception {
	// config.txt를 읽어서 Properties에 저장
	Properties p = new Properties();
	p.load(new FileReader("config.txt"));

	// 클래스 객체(설계도)를 얻어서
	Class clazz = Class.forName(p.getProperty("car"));
	return (Car)clazz.newInstance(); // 객체를 생성해서 반환
}
```

- Properties를 사용해서 외부에서 클래스 정보를 얻어오면 **변경 포인트를 줄일 수 있다.**
- 코드가 변경되면 테스트가 필수인데 코드가 변경되지 않는다는 것은 굉장한 이점. (코드 변경 최소화)
- OOP는 **변경에 유리한 코드를 작성하기 위해 나온 개념**.
- **분리**
    1. 변하는 것과 변하지 않는 것
    2. 관심사
    3. 중복코드

### 3. 객체 컨테이너(Application Context) 만들기

- 객체 컨테이너는 객체 저장소이다.
- 클래스 안에 Map을 가지고 있다. Map이 객체 저장소.
- Properties(String, String)에 있는 내용을 Map(String, Object)로 변환. 객체를 저장하기 위해서.

### 4. 자동 객체 등록하기 - Component Scanning

- class 앞에 @Component 를 붙이면 패키지 내에 @Component 붙은 클래스를 찾아서 객체 생성해서 map에 저장.
- guava 라이브러리를 이용해서 작성. 자바 리플렉션을 더 쉽게 도와주는 라이브러리.

### 5. 객체 찾기 - by Name, by Type

### 6. 객체를 자동 연결하기 - @Autowired

- 객체 저장소에 객체가 등록되어 있을 때, 수동으로 객체 참조를 연결해주는 것이 아니라 자동으로 하는 것이 @Autowired
- @Autowired 어노테이션이 붙어 있으면 객체 저장소를 훑어서 해당하는 객체를 연결해주는 것이다.
- @Autowired는 ByType으로 찾아서 연결해주는 것

### 7. 객체를 자동 연결하기 - @Resource

- @Resource는 ByName으로 찾아서 연결해주는 것
- value를 instanceOf로 찾는 것

# 02. Spring DI 활용하기 - 실습



# 03. Spring DI 활용하기 - 이론
### 1. 빈(bean)이란?

- JavaBeans - 재사용 가능한 컴포넌트, 상태(iv), getter&setter, no-args constructor
- Servlet & JSP bean - MVC의 Model, EL, scope, JSP container가 관리
    - 자바 빈을 MVC 모델에서 데이터 전달할 때 사용함. (Data 전달 목적으로)
    - JSP 컨테이너에 담아서 관리하는 모든 객체를 Bean이라고 함
- EJB (Enterprise Java Beans) - 복잡한 규칙, EJB container가 관리 - 빈의 생성과 소멸을 관리
- Spring Bean - POJO(Plain Old Java Object) - 단순, 독립적, Spring container가 관리
    - 로드 존슨 - 재야의 고수. 서버 쪽 어플리케이션 전문가. 심플하게 할 수 있다고 보여줌.
    - EJB의 복잡한 규칙을 가진 Bean을 심플하게 만든 것.
    - POJO - 원래 예전부터 사용하던 자바 객체. EJB의 반대.

### 2. BeanFactory와 ApplicationContext

- Bean - Spring Container가 관리하는 객체. POJO.
    - 스프링 컨테이너
    - XML 문서를 만들고 bean 태그를 만들면 스프링 컨테이너가 읽어들여서 객체를 만든다.
    - 맵에다가 빈이름, 빈객체 주소를 가지고 있는 것.
- Spring container - Bean 저장소. Bean을 저장, 관리(생성-Life-Cycle, 소멸, 연결)
    1. BeanFactory - Bean을 생성, 연결 등의 기본 기능을 정의
    2. ApplicationContext - BeanFactory를 확장해서 여러 기능을 추가 정의
        - ApplicationContext와 Spring container는 비슷한 것.

### 3. ApplicationContext의 종류

- 다양한 종류의 ApplicationContext 구현체를 제공
    - AC의 종류
        - XML - 텍스트 문서
            - non-Web : GenericXmlApplicationContext
            - Web : XmlWebApplicationContext
        - Java Config - 자바 코드 - 컴파일러가 체크해주는 장점이 있다.
            - non-Web : AnnotationConfigApplicationContext
            - Web : AnnotationConfigWebApplicationContext

### 4. Root AC와 Servlet AC

- Spring MVC는 웹이고, XML 설정을 사용하면 XmlWebApplicationContext를 사용해야 함.
    1. web.xml에서 ContextLoaderListner 톰캣이 시작할 때 이벤트를 체크해서 XmlWebApplicationContext를 만들어줌.
    2. DispatcherServlet을 서블릿으로 등록하면서 XmlWebApplicationContext를 하나 더 만듦. 설정 파일은 servlet-context.
    - 두 ApplicationContext를 부모자식 관계로 연결해줌. 부모에는 공통으로 쓰이는 빈을 넣어놓고, 자식들에는 개별 빈을 넣어놓으면 된다.

### 5. ApplicationContext의 주요 메소드

- getBean - 빈 얻기
- isPrototype / isSingletone - 프로토타입인지 싱글톤인지 확인
- isTypeMatch - 타입 확인
- containsBean - 빈이 있는지 확인

### 6. IoC와 DI

- 제어의 역전(IoC) - 제어의 흐름을 전통적인 방식과 다르게 뒤바꾸는 것
- 의존성 주입(DI) - 사용할 객체를 외부에서 주입받는 것

### 7. 스프링 애너테이션 - @Autowired

- 인스턴스 변수(iv), setter, 참조형 매개변수를 가진 생성자, 메소드에 적용
    - 생성자의 @Autowired는 생략 가능
- @Autowired는 Spring container에서 타입(byType)으로 빈을 검색해서 참조 변수에 자동 주입(DI)
    - 검색된 빈이 n개이면, 그 중에 참조변수와 이름이 일치하는 것을 주입.
- 주입 대상이 변수일 때, 검색된 빈이 1개 아니면 예외 발생
- 주입 대상이 배열일 때, 검색된 빈이 n개라도 예외 발생 X
- @Autowired(required=false)일 때, 주입할 빈을 못찾아도 예외 발생 X

### 8. 스프링 애너테이션 - @Resource

- @Resource는 Spring container에서 이름(byName)으로 빈을 검색해서 참조 변수에 자동 주입(DI)
- 일치하는 이름의 빈이 없으면, 예외 발생.
- 이름 생략 가능. 생략하면 참조변수명이 빈의 이름이 됨.

### 9. 스프링 애너테이션 - @Component

- <component-scan>로 @Component가 클래스를 자동으로 검색해서 빈으로 등록
- @Component(”superEngine”) 빈 이름 생략 가능. 생략하면 클래스 이름의 첫 글자 소문자로 바꿔서 빈 이름으로 등록
- @Controller, @Service, @Repository, @ControllerAdvice의 메타 애너테이션 - 자동 검색해서 빈 등록

### 10. 스프링 애너테이션 - @Value와 @PropertySource

### 11. 스프링 애너테이션 vs. 표준 애너테이션(JSR-330) - Java Spec Request

- 표준 애너테이션은 자바가 제공, 스프링 애너테이션은 스프링이 제공.
- 스프링 애너테이션에 있는 것을 사용하면 된다.

### 12. 빈의 초기화 - <property>와 setter

- <property>를 이용한 빈 초기화. setter를 이용.
- 프로퍼티 태그를 쓰려면 세터 메소드는 있어야 하지만, 세터를 호출하지 않아도 됨.
- 프로퍼티 태그하고 세터 호출하는 것과 같은 것.

### 13. 빈의 초기화 - <constructor-arg>와 생성자

- <constructor-arg>를 이용한 빈 초기화. 생성자를 이용.
- 생성자가 있어야 이 태그를 사용할 수 있다.

### 14. 빈의 초기화 - <list>, <set>, <map>

# 04. Spring으로 DB 연결하기



# 05. Spring으로 DB 다루기 - TDD