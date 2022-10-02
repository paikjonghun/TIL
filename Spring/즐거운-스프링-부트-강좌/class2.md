# 즐거운 스프링 부트 강좌 02 - 스프링 탄생 배경, Bean이란?, Bean을 관리하는 컨테이너란?

> Youtube 채널 [부부개발단 - 즐겁게 프로그래밍 배우기] 의 즐거운 스프링 부트 강좌를 보며 학습한 것 정리

### Spring 프레임워크를 배우기 위해서

- 등장한 이유를 알기
- Bean이 무엇인지 알기
- Bean은 관리된는 것을 알기

### Spring FW 등장한 이유

- EJB(Enterprise JavaBeans)
    - EJB는 애플리케이션 작성을 쉽게 해준다. (미들웨어보단 쉽다. 굉장히 어려웠다.)
    - EJB는 선언적 프로그래밍 모델
    - 트랜잭션, 보안, 분산컴퓨팅 이런 것들을 굉장히 쉽게 할 수 있다.-
    - EJB를 구동시킬 수 있는 Web Application Server가 등장
    - EJB를 만들어서 WAS에 배포
    - EJB 너무 싫다. (개발도 힘들고, 배포도 힘들고) 라는 의견 등장.

### Expert One-on-One: J2EE Design and Development

- 책 제목이다.
- 저자는 로드 존슨(Rod Johnson)
    - EJB처럼 복잡하게 만들지 말고, 다른 방법으로 만들자.
    - 책에서 스프링 예제 파일을 만들게 됨. Spring을 소개한다.
    - EJB에서 했던 일들을 더욱 간단한 Java Bean이 처리할 수 있게 된다.

### Bean이란?

- Java에서 인스턴스 생성
    - `Book book = new Book();`
    - 프로그래머가 직접 인스턴스를 생성함.
- Bean은 컨테이너가 관리하는 객체를 말한다.
    - 객체의 생명주기를 컨테이너가 관리한다.
    - 객체를 싱글턴으로 만들 것인지, 프로토타입으로 만들 것인지.
    - 개발자가 직접 new로 사용하는 게 아니라 컨테이너에 의해 생성되도록 해야 한다.

### 스프링의 핵심 기능 1.

- 관점지향 컨테이너
    - 빈을 생성, 관리한다.
    - 관점지향(AOP, aspect-oriented programming)

### Book.kava

```java
public class Book { // Book 클래스
    private String title; // title 인스턴스 field(속성)
    private int price; // price 인스턴스 field(속성)
		
		// Book 생성자
    public Book(String title, int price) {
        this.title = title;
        this.price = price;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }
}
```

### Book 클래스의 인스턴스 생성

```java
Book book1 = new Book("즐거운 자바", 20000);
```

1. new Book(”즐거운 자바”, 20000)
    - 생성자가 호출되면 Heap 메모리에 인스턴스가 생성된다.
2. book1은 1.에서 생성한 인스턴스를 참조한다.
    1. book1은 참조변수.(reference variable)

### 프로그래머가 직접 인스턴스를 생성, 사용

```java
public class BookExam01 {
    public static void main(String[] args) {
        Book book = new Book("java", 10000);
        System.out.println("book.getPrice() = " + book.getPrice());
        System.out.println("book.getTitle() = " + book.getTitle());
    }
}
```

### Bean을 만들 때 규칙

- 기본 생성자가 있어야 한다.

### 객체를 싱글톤으로 생성해주고, 자기 자신도 싱글턴인 ApplicationContext

- 컨테이너 역할을 수행한다.
- Spring이 제공하는 컨테이너는 이것보다 훨씬 복잡한 일을 한다.

```java
package exam;

import java.io.FileInputStream;
import java.io.IOException;
import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

public class ApplicationContext {

    // 1. 싱글톤 패턴 적용 - 자기 자신을 참조하는 static 필드를 선언한다.
    private static ApplicationContext instance = new ApplicationContext();

    // 3. 1.에서 만든 인스턴스를 반환하는 static 메소드를 만든다.
    public static ApplicationContext getInstance() {
        return instance;
    }

    private Properties props;
    private Map objectMap;

    // 2. 싱글톤 패턴 적용 - 생성자를 private로 바꾼다.
    private ApplicationContext() {
        props = new Properties();
        objectMap = new HashMap<String, Object>();
        try {
            props.load(new FileInputStream("src/main/resources/Beans.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public Object getBean(String id) throws Exception {

        Object o1 = objectMap.get(id);

        if(o1 == null) {

            String className = props.getProperty(id);
            // class name에 해당하는 문자열을 가지고 인스턴스를 생성할 수 있다.
            // Class.forName(className)은 CLASSPATH로부터 className에 해당하는 클래를 찾은 후 클래스 정보를 반환한다.
            Class clazz = Class.forName(className);

            // ClassLoader를 이용한 인스턴스 생성. 기본생성자가 있어야 한다.
            Object o = clazz.newInstance(); // clazz 정보를 이용해 인스턴스를 생성한다.
            objectMap.put(id, o);
            o1 = objectMap.get(id);
        }

        return o1;
    }
}
```