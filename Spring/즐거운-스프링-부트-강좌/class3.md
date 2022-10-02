# 즐거운 스프링 부트 03. ApplicationContext와 XML설정파일 읽어들이기

> Youtube 채널 [부부개발단 - 즐겁게 프로그래밍 배우기] 의 즐거운 스프링 부트 강좌를 보며 학습한 것 정리

## 1. ApplicationContext

- ApplicationContext는 다양한 인터페이스를 상속받고 있다.
    - **스프링 컨테이너의 핵심 인터페이스**

```java
org.springframework.context
Interface ApplicationContext
```

- 그중에서도 BeanFactory도 ApplicationContext는 상속받는다.

```java
org.springframework.beans.factory
Interface BeanFactory
```

## 2. ApplicationContext를 구현하고 있는 대표적인 클래스

- CLASSPATH에서 XML 설정 파일을 읽어들여 동작한다.

```java
org.springframework.context.support
Class ClassPathXmlApplicationContext
```

## 3. 스프링 프레임워크의 핵심 모듈

- Data Access/Integration
    - JDBC
    - ORM
    - OXM
    - JMS
    - Transactions
- Web
    - Web
    - Servlet
    - Portlet
- AOP
- Instrumentation
- **Core Container(Core Container가 컨테이너를 구성하는 핵심이면서 스프링 프레임워크의 핵심!)**
    - Beans
    - Core
    - Context
    - Expression Language
    - Test

### Gradle에서 아래의 라이브러리를 추가한다.

```java
implementation group: 'org.springframework', name: 'spring-context', version: '5.3.23'
```

## 4. Spring ApplicationContext를 사용해보자

```java
package com.example.spring02;

import exam.Book;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringApplicationContextExam {
    public static void main(String[] args) {

        // 인스턴스를 인터페이스 타입으로 참조할 수 있다.
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
//        Book book1 = (Book) context.getBean("book1");
        Book book1 = context.getBean("book1", Book.class);
        book1.setTitle("즐거운 Spring Boot");
        book1.setPrice(5000);

        System.out.println("book1.getTitle() = " + book1.getTitle());
        System.out.println("book1.getPrice() = " + book1.getPrice());

        Book book2 = (Book) context.getBean("book1");
        System.out.println("book2.getTitle() = " + book2.getTitle());

    }
}
```

## MyService & MyDao 클래스 다이어그램

- 연관관계
    - MyService가 MyDao를 가진다.
    - setter 주입
    
    ```java
    MyService myService = new MyService();
    MyDao myDao = new MyDao();
    myService.setMyDao(MyDao);
    ```
    
    - 생성자 주입
    
    ```java
    MyService myService = new MyService(new MyDao());
    ```
    

## Spring 설정으로 주입

- 아래 세 줄의 코드를 xml파일 설정으로 한다면,

```java
MyService myService = new MyService();
MyDao myDao = new MyDao();
myService.setMyDao(MyDao);
```

- 아래 xml 코드처럼 된다.

```xml
<bean id="myService" class="com.example.spring02.component.MyService">
    <property name="myDao" ref="myDao"/>
</bean>
<bean id="myDao" class="com.example.spring02.component.MyDao"></bean>
```