> 패스트캠퍼스 - 스프링의 정석(남궁성) 강의를 보며 배운 것들 정리

# 01. 원격 프로그램의 실행

### 1. 로컬 프로그램 실행

- 로컬 프로그램이 노트북에 있을 때, 커맨드 라인에서 실행할 때는 java.exe(자바 인터프리터)가 main() 메소드를 호출함.
- main() 메소드가 static 이라서 객체 생성할 필요가 없다.

### 2. 원격 프로그램 실행

- 다른 컴퓨터에 있는 프로그램을 실행하는 것. 필요한 것은 브라우저와 WAS.
- 브라우저에서 URL 입력하면, Tomcat이 요청을 받아서 프로그램을 실행함.
    1. 프로그램 등록. `@Controller` 애너테이션을 클래스 앞에 붙여주기
    2. URL과 프로그램을 연결. `@RequestMapping` 을 메소드 앞에 붙여주기. 메소드 이름은 중요하지 않음.

- 스프링 프레임워크는 자바의 Reflection API를 이용해서 객체를 생성하기 때문에 접근제어자 영향을 받지 않을 수 있음.

# 03. HTTP 요청과 응답

### 1. HttpServletRequest

- 톰캣이 HttpServletRequest 객체에 요청 데이터들을 담아서 메소드에 넘겨준다. 메소드 매개변수로 선언하면 자동으로 알아서 넣어준다.

### 2. HttpServletRequest의 메소드

- getScheme() - http를 준다.
- getServerName()
- getServerPort()
- getContextPath()
- getServletPath()
- getQueryString() - 쿼리 스트링. 값을 전달할 때 사용.

- 서버가 제공하는 리소스
    - 동적 리소스
        - 프로그램이 생성하는 리소스 결과
        - 스트리밍
    - 정적 리소스
        - 이미지
        - js
        - css
        - html

### 3. 클라이언트와 서버

- 클라이언트(client) - 서비스를 요청하는 애플리케이션
- 서버(server) - 서비스(service)를 제공하는 애플리케이션
- 요청해서 데이터를 주면 서버에서 처리해서 응답을 하는 것

# 05. 클라이언트와 서버

### 1. HttpServletRequest

- 브라우저를 이용해서 URL로 요청을 하면, 해당 톰캣이 객체를 생성해서 정보를 객체에 저장. 그것을 메소드에 매개변수로 제공. 필요한 것만 매개변수로 적어주면 톰캣이 알아서 넘겨준다.

### 2. HttpServletRequest의 메소드

- QueryString의 값을 받으려면 getParameter() 메소드 이용. 키와 밸류가 문자열이라 숫자로 변환하고 싶으면 Integer.parseInt() 같은 메소드 사용.
- Enumeration enum = request.getParameterNames();
- Map paramMap = request.getParameterMap();
- 키값이 같은 경우에는 getParameterValues()로 받을 수 있음.
    - String[] yearArr = request.getParameterValues(”year”);

## 클라이언트와 서버

### 1. 클라이언트와 서버

- 클라이언트(client) : 서비스를 요청하는 애플리케이션(or 컴퓨터)
- 서버(server) : 서비스(service)를 제공하는 애플리케이션(or 컴퓨터)

### 2. 서버의 종류 - 어떤 서비스를 제공하느냐에 따라 달라짐

- Email Server - 이메일 서비스를 제공하는 서버
- File Server - 파일을 제공하는 서버
- Web Server - Web을 제공하는 서버 - 브라우저를 통해서 받을 수 있는 모든 서비스를 제공. 문서, 동영상, 이미지 등

### 3. 서버의 포트

- 한 대의 PC에 여러가지 서버가 있을 때, 어떤 서버에 대한 요청인지 구분하려면 IP주소만으로는 구분하기 어렵다. 그래서 필요한 것이 포트번호. 웹서버는 기본 포트번호가 80. 기본 포트번호는 생략 가능.

### 4. 웹 애플리케이션 서버(WAS)란?

- 웹 애플리케이션 서버(WAS) : 웹 애플리케이션을 서비스하는 서버
    - 서버에 프로그램을 설치해놓고, 클라이언트가 서버의 프로그램을 사용할 수 있도록 한 것. 그것이 WAS. 톰캣. 원격 프로그램을 호출.
    

### 5. Tomcat의 내부 구조

- 브라우저로 요청을 하면, 톰캣에 요청이 들어오고, 기다리고 있던 스레드 풀에서 한가한 스레드가 요청을 처리. Server(Tomcat) 안에 Service가 있음. Connector가 처리하고. Service 안에 Engine 안에 하나의 Host 안에 Context(하나의 Web App. STS 프로젝트 하나.) 안에 서블릿이 있음.
- 서블릿은 작은 서버프로그램.

### 6. Tomcat의 설정 파일 - server.xml, web.xml

- 톰캣설치경로/conf/server.xml : Tomcat 서버 설정 파일
- 톰캣설치경로/conf/web.xml : Tomcat의 모든 web app의 공통 설정
- 웹앱이름/WEB-INF/web.xml : web app의 개별 설정

# 07. HTTP 요청과 응답 - 이론

### 1. 프로토콜(protocol)이란?

- 서로 간의 통신을 위한 약속, 규칙
- 주고 받을 데이터에 대한 형식을 정의한 것

### 2. HTTP(Hyper Text Transfer Protocol)이란?

- 단순하고 읽기 쉽다.(Human readable) - 텍스트 기반의 프로토콜
    - 텍스트 기반의 전송 규칙
- 상태를 유지하지 않는다.(stateless) - 클라이언트 정보를 저장X. 서버는 클라이언트 요청을 구별할 수 없다. 보완하기 위해 사용하는 것이 쿠키와 세션.(클라이언트 구별 가능)
- 확장 가능하다. - 커스텀 헤더(header) 추가 가능
    - 헤더는 대소문자 구분 X.
    - 공백 무시됨.
    - 헤더는 N개 나올 수 있음.
    - 표준에 정의되지 않은 헤더를 추가 가능(custom header)

### 3. HTTP 메시지

- 편지와 비슷하다.
- 요청 메시지와 응답 메시지를 보내는 것과 같다.
- 브라우저에서 URL을 입력하고 엔터를 치면, 브라우저는 HTTP 요청 메시지를 자동으로 만들어서 보낸다.
- 서버에서 HTTP 요청 메시지를 받고 처리를 해서 HTTP 응답 메시지를 보내 브라우저에 출력됨.

### 4. HTTP 메시지 - 응답 메시지

- HTTP/1.1 200 OK - 상태 라인
    - 상태코드
        - 1xx - Informational - HTTP/1.1에서 추가. 정보교환이 목적. 잘 안씀.
        - 2xx - Success - 성공
        - 3xx - Redirect - 다른 URL 요청
        - 4xx - Client Error
            - 404 Not Found - 클라이언트가 요청을 잘못함.
        - 5xx - Server Error
- 헤더 - 헤더가 N줄이 될 수 있음. 그래서 중간에 빈줄로 헤더와 바디를 구분
    - Content-Length : 44
    - Content-Type : text/html
    - Date : Sat, 20 Oct 2021 19:03:38 GMT
    

### 5. HTTP 메시지 - 요청 메시지

- GET, POST을 요청 메소드라고도 함.
- **HTTP 요청 메시지 - GET**
    - 단순히 리소스를 얻어오기 위한 것. Read.
    - 바디가 없음.
    - 데이터를 보낼 게 있으면 쿼리스트링으로 데이터를 보낼 수 있음.
- **HTTP 요청 메시지 - POST**
    - Write. 글쓰기, 로그인, 회원가입, 파일첨부 등의 목적은 POST.
    - 바디가 있어서 서버에 전송할 데이터를 바디에 담을 수 있음.

### 6. HTTP 메소드 - GET, POST

- GET
    - 서버의 리소스를 가져오기 위해 설계
    - Body가 없음. 그래서 QueryString을 통해 데이터를 전달(소용량)
    - URL에 데이터 노출되므로 보안에 취약
    - 데이터 공유에 유리
    - ex. 검색 엔진에서 검색 단어 전송에 이용
- POST
    - 서버에 데이터를 올리기 위해 설계됨
    - 전송 데이터 크기의 제한이 없음(대용량)
    - 데이터를 요청 메시지의 body에 담아 전송
    - 보안에 유리, 데이터 공유에는 불리 - HTTP + TLS(암호화) = https 하기 때문에 보안에 유리. POST라서 보안에 유리한 것이 아니다.
    - ex. 게시판에 글쓰기, 로그인, 회원가입

# 08. 텍스트와 바이너리, MIME, Base64

### 7. 텍스트 파일 vs. 바이너리 파일

- 바이너리 파일 : 문자와 숫자가 저장되어있는 파일
    - 데이터를 있는 그대로 읽고 쓴다.
- 텍스트 파일 : 문자만 있는 저장되어 있는 파일
    - 숫자를 문자로 변환 후 쓴다.
    

### 8. MIME(Multipurpose Internet Mail Extensions)

- 텍스트 기반 프로토콜에 바이너리 데이터 전송하기 위해 고안. HTTP의 Content-Type 헤더에 사용. 데이터의 타입을 명시
    - text - 텍스트를 포함하는 모든 문서
    - image - 모든 종류의 이미지
    - audio - 모든 종류의 오디오 파일
    - video - 모든 종류의 비디오 파일
    - application - 모든 종류의 이진 데이터

### 9. Base 64 - 64진법

- 바이너리 데이터를 텍스트 데이터로 변환할 때 사용
- 64진법은 ‘0’~’9’, ‘A’~’Z’, ‘a’~’z’, ‘+’. ‘/’ 모두 64개의 문자로 구성
- 데이터를 표현할 때 64개 문자로 표현. 6bit

# 09. 관심사의 분리와 MVC 패턴

### 1. 관심사의 분리 Separation of Concerns

- 관심사(concern) : 해야할 작업
    - 입력 / 계산 / 출력 등
- OOP 5대 설계 원칙 - SOLID
    - SRP - 단일 책임의 원칙
        - 하나의 메소드는 하나의 책임(관심사)만 진다.
- 객체지향적으로 좋은 설계를 하려면 ‘코드의 분리’를 잘 해야함.
    - 분리에는 `관심사의 분리`가 있고, `변하는 것과 변하지 않는 것의 분리`, `공통 코드의 분리`가 있다.

### 2. 공통 코드의 분리 - 입력의 분리

### 3. 출력(view)의 분리 - 변하는 것과 변하지 않는 것의 분리

### 4. MVC 패턴

- DispatcherServlet
    - 입력 & 변환
    - 모델 생성
- Controller
    - 유효성 검사
    - 처리
    - Model에 작업 결과 저장
    - 작업 결과를 보여줄 view의 이름을 반환

### 5. ModelAndView

- Model과 View를 합친 것

### 6. 컨트롤러 메소드의 반환타입

- String : 뷰 이름을 반환
- void : 맵핑된 url의 끝단어가 뷰 이름
- ModelAndView : Model과 뷰 이름을 반환

# 13. 서블릿과 JSP

### 1. 서블릿과 컨트롤러의 비교

JSP와 서블릿은 거의 같은 것. 

서블릿와 컨트롤러는 유사하지만 컨트롤러가 더 발전된 형태.

서블릿을 발전시킨 것이 스프링.

스프링이 서블릿을 이용하기도 함.(DispatcherServlet)

@WebServlet = @Controller + @RequestMapping

### 2. 서블릿의 생명주기

### 3. JSP(Java Server Pages)란?

- 서블릿과 비슷하다. JSP로 작성하면, 자동으로 서블릿으로 변환됨.
- Java in HTML

### 4. JSP와 서블릿의 비교

- `<%! 클래스 영역 %>` - iv, cv 선언 → 클래스 안으로 들어감
- `<% 메소드 영역 - service()의 내부 %>`

### 5. JSP의 호출 과정

1. *.jsp요청이 들어오면 jspServlet이 받고, 서블릿 인스턴스가 존재하는지 확인.
2. 없으면, .jsp를 서블릿으로 변환하고 컴파일해 class파일을 만들고 객체를 생성함.
3. 서블릿 객체가 만들어지면 Service() 호출.

3-1. 서블릿 인스턴스 존재하면 Service() 바로 호출.

- 서블릿 : lazy-init. 늦은 초기화. 요청이 오면 초기화를 함.
- Spring : early-init. 빠른 초기화. 요청이 오지 않아도 객체를 만들고 초기화를 함.

### 6. JSP와 서블릿으로 변환된 JSP 비교

### 7. JSP의 기본 객체

- 생성 없이 사용할 수 있는 객체
    - request는 기본 객체.
    - response, pageContext, session, out, page 등

### 8. 유효 범위(scope)와 속성(attribute)

- HTTP 특징 - 상태 정보를 저장하지 않는다. Stateless.
- 저장소
    - pageContext
        - pageContext의 범위는 jsp 페이지 내부
    - apllication
        - 유효 범위는 Servlet Context 전체
        - 모든 jsp에서 접근 가능
    - session
        - 서버(메모리) 부담이 크다. 사용을 최소화 하는 것이 좋다.
        - 세션 객체를 저장소로 이용하는게 프로그래밍하기에는 편리하다.
        - 클라이언트 마다 하나씩 존재. 클라이언트가 접근하는 페이지에서 다 접근할 수 있음.
    - request
        - request객체가 Map을 가지고 있는 것.
        - 보통은 한 request 객체가 한 jsp 페이지에서 끝나는데, 다른 jsp로 넘겨줄 수 있다.
        - request에 저장을 하고 다른 jsp로 보낼 수 있다.
        - 요청이 처리되는 동안 존재.

- pageContext
    - 유효 범위 - 1개 jsp 페이지
- request
    - 유효 범위 - 1+개 jsp 페이지
- session
    - 유효 범위 - n개 jsp 페이지
- application
    - 유효 범위 - context 전체

- 속성 관련 메소드
    - setAttribute - 저장
    - getAttribute -  읽기
    - removeAttribute - 삭제
    - getAttributeNames() - 모든 속성의 이름을 반환

### 9. URL 패턴

- @WebServlet으로 서블릿을 URL에 매핑할 때 사용.
    1. exact mapping - 정확히 일치하는 것. 우선순위 가장 높다. 이 패턴이 없으면 두 번째 우선순위로.
    2. path mapping - 경로매핑. 경로에 와일드카드 쓴 것.
    3. extension mapping - 확장자 매핑.
    4. default mapping - 디폴트 매핑. 모든 주소하고 다 매핑됨.
- loadOnStartup - 미리 초기화

web.xml 을 보면, DispatcherServlet을 appServlet이라는 이름으로 등록. 

서버 안에 web.xml은 전체설정, 프로젝트 안에 web.xml은 개별 설정.

### 10. EL (Expression Language)

- <%=값%> 이렇게 쓰던 것을 ${값} 이렇게 쓰는 것이 EL
- EL은 간단하고 편리하게 해준다.

### 11. JSTL (JSP Standard Tag Library)

- 태그 라이브러리.
- jsp 에서 if문을 쓰려면 코드들이 쪼개져서 번거롭다.
- 값 찍는 것을 간편하게 태그화 한 것.

### 12. Filter

- 공통적인 요청 전처리와 응답 후처리에 사용. 로깅, 인코딩 등.

# 17. @RequestParam과 @ModelAttribute

### 1. @RequestParam

- 요청의 파라미터를 연결할 매개변수에 붙이는 애너테이션
- String과 기본형은 애너테이션을 생략할 수 있다.
- 애너테이션 생략하면 기본값은 `@RequestParam(name=”매개변수이름”, required=false)`
- @RequestParam만 붙이면 `@RequestParam(name=”year”, required=true)`

### 2. @ModelAttribute

- 적용 대상을 Model의 속성으로 자동 추가해주는 애너테이션
- 반환 타입 또는 컨트롤러 메소드의 매개변수에 적용 가능

- 컨트롤러의 매개변수에 붙일 수 있는 애너테이션
    - @RequestParam
        - String과 기본형은 애너테이션 생략 가능
    - @ModelAttribute
        - 참조형 매개변수는 애너테이션 생략 가능
        

### 3. WebDataBinder

1. 타입 변환
    1. 타입이 일치하지 않을 때, 타입 변환을 해줌.
    2. 결과와 에러를 BindingResult에 저장.
2. 데이터 검증
    1. 타입 변환 완료 후 데이터 검증.
    2. 결과와 에러를 BindingResult에 저장
    3. 컨트롤러에 BindingResult 넘겨줄 수 있음.

# 19. 회원가입 화면 작성하기

- servlet-context.xml : web관련 설정파일
- root-context.xml : non-web관련 설정파일

- 주소칠 때 resources를 빼주려면,
    - servlet-context 안에 resources 태그 안에 mapping에서 resource 빼기
        
        ```java
        <resources mapping="/**" location="/resources/" />
        ```
        

- <c:url> 가 하는 일
    1. context root 자동 추가
    2. session id 자동 추가

# 20. @GetMapping, @PostMapping

- Get 방식으로 회원가입 할 수 없게 만들기
    - RequestMapping 사용하거나 PostMapping 사용. 하지만 PostMapping은 스프링 4.3부터 사용 가능
    
    ```java
    @RequestMapping(value="/register/save", method=RequestMethod.POST)
    
    @PostMapping("/register/save") // 4.3부터 PostMapping 쓸 수 있게됨
    ```
    

- servlet-context.xml 에서 <view-controller /> 추가. 정확하게는 <mvc:view-controller /> 인데 mvc 생략 가능. GET 방식 요청에서 view와 controller 단순 연결 가능.

```xml
<view-controller path="/register/add" view-name="registerForm" />
```

- URLEncoder.encode
    - GET방식으로 요청을 보낼 때, URL에 들어갈 내용을 인코딩하는 것. jsp에서 받으려면 decode 해야 함.
    
    ```java
    String msg = URLEncoder.encode("id를 잘못 입력하셨습니다.", "utf-8");
    return "redirect:/register/add?msg=" + msg; // URL 재작성(rewriting)
    ```
    

### 1. @GetMapping, @PostMapping

- @ReuqestMapping 대신 @GetMapping, @PostMapping 사용 가능
- Mapping 된 URL이 같으면 충돌이 나는데 메소드가 다르면 괜찮다.

### 2. 클래스에 붙이는 @RequestMapping

- 맵핑될 URL의 공통 부분을 @RequestMapping으로 클래스에 적용

### 3. @RequestMapping의 URL패턴

- ?는 한 글자, *는 여러 글자, **는 하위 경로 포함. 배열로 여러 패턴 지정

### 4. URL 인코딩 - 퍼센트 인코딩

- URL에 포함된 non-ASCII 문자를 문자 코드(16진수) 문자열로 변환
- URLEncoder.encode()
- URLDecoder.decode()

# 22. redirect와 forward

### 1. redirect와 forward의 처리 과정 비교. JSP를 통해서.

- redirect
    1. /ch2/write.jsp 요청
    2. 302 응답. 3xx 응답 코드는 Redirct 하라는 것. 다른 URL로 재요청하라는 것. 그러고 Location을 줌. 응답 헤더는 있고, 응답 바디는 없다.
    3. 브라우저가 자동으로 Location대로 새로운 요청을 함. 그래서 요청은 2회. 응답도 2회. request 객체도 2개. redirect에 의한 요청은 GET 요청.
    
- forward
    1. /ch2/write.jsp 요청
    2. write.jsp가 다른 jsp에게 request 객체(사용자 요청 내용)를 전달(forward)을 함
    3. 전달 받은 jsp가 응답.

### 2. RedircetView

- 응답헤더 만들어냄.

### 3. JstlView

- 뷰 이름을 받아서 InternalResourceViewResolver가 이름을 해석. 실제로 어떤 view인지. 그 view를 JstlView에게 넘겨주고, 해당 jsp에게 Model을 넘겨줌.
- jsp 파일을 처리하기 위한 것.

### 4. InternalResourceView

- forward로 시작하는 view 이름을 반환하면, DispatcherServlet이 InternalResourceView에게 전달. 내부적으로 요청하는 것. forwarding은 InternalResourceView가 처리

# 23. 쿠키(Cookie)란?

### 1. 쿠키란?
- 이름과 값의 쌍으로 구성된 정보. 아스키 문자만 가능(한글은 URL 인코딩 해야함)
- 도메인, 경로, 유효기간 등을 저장 가능.
- 서버에서 생성 후 전송, 브라우저에 저장. 유효기간 이후 자동 삭제.
- 서버에 요청시 domain, path가 일치하는 경우에만 자동 전송.

### 2. 쿠키의 작동 과정
1. 클라이언트가 서버에 요청히면 서버는 쿠키를 만들어서 응답헤더에 쿠키 정보를 담아서 클라이언트에 응답.
2. 서버가 보내준 쿠키가 브라우저에 저장됨.
3. 클라이언트가 서버에 다시 요청을 하게되면, 자동으로 요청헤더에 쿠키가 따라가게 되어있음.
   
- 쿠키는 클라이언트 식별 기술이다. 서버는 요청 오는 것에 대한 응답만 하니까, 경우에 따라서는 클라이언트를 구별해야할 때가 있음. 그럴 때 쿠키를 사용함.

### 3. 쿠키의 생성
```java
    Cookie cookie = new Cookie ("id", "asdf"); // 쿠키 생성
    cookie.setMaxAge(60*60*24); // 유효기간 설정(초)
    response.addCookie(cookie); // 응답에 쿠키 추가
```

### 4. 쿠키의 삭제와 변경
- 쿠키의 삭제
```java
    Cookie cookie = new Cookie("id", ""); // 변경할 쿠키와 같은 이름 쿠키 생성
    cookie.setMaxAge(0); // 유효 기간을 0으로 설정(삭제)
    response.addCookie(cookie); // 응답에 쿠키 추가
```

- 쿠키의 변경
```java
    Cookie cookie = new Cookie("id", ""); // 변경할 쿠키와 같은 이름 쿠키 생성.
    cookie.setValue(URLEncoder.encode("남궁성")); // 값의 변경
    cookie.setDomain("www.fastcampus.co.kr"); // 도메인의 변경
    cookie.setPath("/ch2"); // 경로의 변경
    cooke.setMaxAge(60*60*24*7); // 유효기간의 변경
    response.addCookie(cookie); // 응답에 쿠키 추가
```

### 5. 쿠키 읽어오기
```java
    Cookie[] cookies = request.getCookies(); // 쿠키 읽기
    for(Cookie cookie : cookies) {
        String name = cookie.getName();
        String value = cookie.getValue();

        System.out.printf("[cookie]name=%s, value=%s%n", name, value);
    }
```

# 24. 세션(Session) - 이론

### 1. 세션이란?

- 서로 관련된 요청들을 하나로 묶은 것 - 쿠키를 이용.
    - 원래 요청은 독립적. (서로 요청간의 관계가 없다.) 이것들을 묶은 것이 세션.
- browser마다 개별 저장소(session 객체)를 서버에서 제공.
    - 쿠키가 브라우저에 저장되고, 세션은 서버에 저장됨.
    
- Session
    - a collection of related HTTP transactions made by one browser to one server
    

### 2. 세션의 생성 과정

1. 처음에 브라우저가 요청을 하면 서버는 무조건 세션 객체(저장소)를 만든다. 세션 객체마다 가지고 있는 것은 세션 ID. 
2. 세션 ID가 담긴 쿠키를 응답에 담아 보냄.
3. 브라우저에 쿠키가 저장됨. SESSIONID가 요청에 포함되어서 전송됨.

- 서버는 응답헤더에 Set-Cookie라는 응답헤더를 이용해서 세션ID를 보내고, 브라우저에 쿠키가 만들어지고 다음 요청부터는 JSESSIONID를 쿠키에 담아 요청하게 됨.

### 3. 세션 객체 얻기

- 쿠키는 브라우저마다 다른 쿠키를 저장하기 때문에, 한 컴퓨터 내에서 서로 다른 브라우저여도 다른 세션ID를 서버로부터 받는다.

### 5. 세션의 종료

1. 수동 종료

```java
HttpSession session = request.getSession(); // 1. 세션을 즉시 종료
session.invalidate();
session.setMaxInactiveInterval(30*60); // 2. 예약 종료(30분 후)
```

1. 자동 종료 - web.xml

```xml
<session-config>
	<session-timeout>30</session-timeout>
</session-config>
```

### 6. 쿠키 vs. 세션

- 쿠키
    - 브라우저에 저장
    - 서버 부담 X
    - 보안에 불리
    - 서버 다중화에 유리
- 세션
    - 서버에 저장
    - 서버 부담 O
    - 보안에 유리
    - 서버 다중화에 불리

# 25. 세션(Session) - 실습(1)

### 1. 게시판 이용시, 미로그인이면 로그인 화면 이동

# 26. 세션(Session) - 실습(2)

### 2. 로그인 후, 게시판으로 이동

### 3. 세션을 시작할까? session = "true" or session = "false" ?
- 세션은 서버에 부담이 되기 때문에 최대한 세션 유지기간이 짧은 것이 좋다. 그래서 세션이 필요없는 홈화면이나 로그인 화면에서는 세션을 시작되지 않도록 하면 좋다.

# 28. 예외처리 - 이론

### 1. @ExceptionHandler 와 @ControllerAdvice

- 예외 처리를 위한 메소드를 작성하고 @ExceptionHandler를 붙인다.
- @ControllerAdvice로 전역 예외 처리 클래스 작성 가능(패키지 지정 가능)
- 예외 처리 메소드가 중복된 경우, 컨트롤러 내의 예외 처리 메소드가 우선

### 2. @ResponseStatus

- 응답 메시지의 상태 코드를 변경할 때 사용
    1. 예외 처리 메소드에서 사용
    2. 사용자 정의 예외 클래스에서 사용

### 3. <error-page> - web.xml

- 상태 코드별 뷰 맵핑

```xml
<error-page>
	<error-code>400</error-code>
	<location>/error400.jsp</location>
</error-page>
<error-page>
	<error-code>500</error-code>
	<location>/error500.jsp</location>
</error-page>
```

### 4. SimpleMappingExceptionResolver

- 예외 종류별 뷰 맵핑에 사용. servlet-context.xml에 등록
- 예외 종류와 에러뷰를 연결.

```xml
<beans:bean class="org.springframework.web.servlet.handler.SimpleMaapingExceptionResolover">
	<beans:property name="defaultErrorView" value="error"/>
	<beans:property name="exceptionMappings">
		<beans:props>
			<beans:prop key="com.fastcampus.ch2.MyException">error400</beans:prop>
		</beans:props>
	</beans:property>
	<beans:property name="statusCodes">
		<beans:props>
			<beans:prop key="error400">400</beans:prop>
		</beans:props>
	</beans:property>
</beans:bean>
```

### 5. ExceptionResolver

- 클라이언트에서 요청 → DispatcherServlet → Controller에서 예외 발생. → try-catch로 처리할 수 있지만 없으면, 호출한 곳으로 예외를 전달. → DispatcherServlet은 handlerExceptionResolvers에서 예외를 처리할 수 있는지 확인.(예외 처리 기본 전략)

### 6. 스프링에서의 예외 처리

- 컨트롤러 메소드 내에서 try-catch로 처리
- 컨트롤러에 @ExceptionHandler메소드가 처리
- @ControllerAdvice 클래스의 @ExceptionHandler 메소드가 처리
- 예외 종류별로 뷰 지정 - SimpleMappingExceptionResolver
- 응답 상태 코드별로 뷰 지정 - <error-page>

# 29. DispatcherServlet

### 1. DispatcherServlet 이란?

- DispatcherServlet이 전처리를 해준다.

### 2. Spring MVC의 요청 처리 과정

1. 클라이언트에서 요청
2. DispatcherServlet에서 HandlerMapping이 등록된 URL과 일치하는 메소드를 반환
3. HandlerMapping에서 받아서 HandlerAdapter를 거쳐서 컨트롤러를 호출(느슨한 연결. 변경에 유리.)하고 View name을 반환
4. ViewResolver에서 View name을 받아 주소를 만들어 반환.
5. DispatchServlet이 JstlView(인터페이스)를 통해서 Model을 보내면서 해당 View를 호출함

### 3. DispatcherServlet의 소스 분석

- DispatcherServlet.class는 spring-webmvc-5.0.7.RELEASE.jar에 포함
- 소스 파일 위치 - org/springframework/web/servlet/DispatcherServlet.java
- 기본 전략 - org/springframework/web/servlet/DispatcherServlet.properties

# 30. 데이터의 변환과 검증

### 1. WebDataBinder

- 요청을 URL로 했을 때, 데이터가 parameter Map으로 바뀐다.
- WebDataBinder가 타입을 변환하고 BindingResult에 저장, 그리고 데이터 검증을 한다.

### 2. RegisterController에 변환 기능 추가하기 - 실습

- @InitBinder

### 3. PropertyEditor

- PropertyEditor - 양방향 타입 변환 (String → 타입, 타입 → String)
    - 특정 타입이나 이름의 필드에 적용 가능
- 디폴트 PropertyEditor - 스프링이 기본적으로 제공
- 커스텀 PropertyEditor - 사용자가 직접 구현. PropertyEditorSupport를 상속하면 편리.
- 모든 컨트롤러 내에서의 변환 - WebBindingInitalizer를 구현 후 등록
- 특정 컨트롤러 내에서의 변환 - 컨트롤러에 @InitBinder가 붙은 메소드를 작성

### 4. Converter와 ConversionService

- Converter - 단방향 타입 변환(타입A → 타입B)
    - PropertyEditor의 단점을 개선(stateful → stateless)
- ConversionService - 타입 변환 서비스를 제공. 여러 Converter를 등록 가능
    - WebDataBinder에 DefaultFormattingConversionService이 기본 등록
    - 모든 컨트롤러 내에서의 변환 - ConfigurableWebBindingInitializer를 설정해서 사용
    - 특정 컨트롤러 내에서의 변환 - 컨트롤러에 @InitBinder가 붙은 메소드를 작성

### 5. Formatter

- Formatter - 양방향 타입 변환(String → 타입, 타입 → String)
    - 바인딩할 필드에 적용 - @NumberFormat, @DateTimeFormat

### 6. Validator란?

- 객체를 검증하기 위한 인터페이스. 객체 검증기(validator) 구현에 사용

### 7. Validator를 이용한 검증 - 수동

```java
UserValidator userValidator = new UserValidator();
userValidator.validate(user, result); // validator로 검증

if(result.hasErrors()) { // 에러가 있으면,
	return "registerForm";
}
```

### 8. Validator를 이용한 검증 - 자동

```java
@InitBinder
public void toDate(WebDataBinder binder) {
	SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
	binder.registerCustomEditor(Date.class, new CustomDateEditor(df, false));
	
	binder.setValidator(new UserValidator()); // validator를 WebDataBinder에 등록
}

@PostMapping("/register/add") // 신규 회원 등록
public String save(Model m, @Valid User user, BindingResult result) {
	if(result.hasErrors()) { // 에러가 있으면
		return "registerForm";
	}
}
```

### 9. 글로벌 Validator

- 하나의 Validator로 여러 객체를 검증할 때, 글로벌 Validator로 등록.
- 글로벌 Validator로 등록하는 방법

```java
<annotation-driven validator="globalValidator"/>
<beans:bean id="globalValidator" class="com.fastcampus.ch2.GlobalValidator" />
```

- 글로벌 Validator와 로컬 Validator를 동시에 적용하는 방법

```java
@InitBinder
public void toDate(WebDataBinder binder) {
	SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
	binder.registerCustomEditor(Date.class, new CustomDateEditor(df, false));
	
	binder.addValidators(new UserValidator()); // validator를 등록
}
```

### 10. MessageSource

- 다양한 리소스에서 메시지를 읽기 위한 인터페이스
    
    ```java
    public interface MessageSource {
    	String getMessage(String code, object[] args, String defaultMessage, Locale locale);
    	String getMessage(String code, object[] args, Locale locale) throws NoSuchMessageException;
    	String getMessage(MessageSourceResolvable resolvable, Locale locale) throws NoSuchMessageException;
    }
    ```
    
- 프로퍼티 파일을 메시지 소스로 하는 ResourceBundleMessageSource를 등록(servlet-context.xml)
    
    ```xml
    <beans:bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    	<beans:property name="basnames">
    		<beans:list>
    			<beans:value>error_message</beans:value><!-- /src/main/resources/error_message.properties -->
    		</beans:list>
    	</beans:property>
    	<beans:property name="defaultEncoding" value="UTF-8"/>
    </beans:bean>
    ```
    
- [error_message.properties]
    
    ```xml
    required=필수 항목입니다.
    required.user.pwd=사용자 비밀번호는 필수 항목입니다.
    invalidLength.id=아이디의 길이는 {1}~{2} 사이어야 합니다.
    ```
    

### 11. 검증 메시지의 출력

- 스프링이 제공하는 커스텀 태그 라이브러리를 사용
    
    ```html
    <%@ taglib uri="http://www.springframework.org/tags/form" prefix="form" %>
    ```
    
- <form> 대신 <form:form> 사용
    
    ```html
    <form:form modelAttribute="user">
    ```
    
    ```html
    <form id="user" action="/ch2/register/add" method="post">
    ```
    
- <form:errors>로 에러를 출력. path에 에러 발생 필드를 지정.(*은 모든 필드의 에러)
    
    ```html
    <form:errors path="id" cssClass="msg"/>
    ```
    
    ```html
    <span id="id.errors" class="msg">필수 입력 항목입니다.</span>
    ```

# 35. IntelliJ 사용법 익히기
- 더블쉬프트 : 검색
- esc : 어느 창에 포커스가 되어있더라도 에디터 창으로 포커스가 이동
- Quick Preview : 프로젝트 창에서 클래스를 스페이스 바 누르면 미리보기
- 에디터 창의 열린 파일 탭간의 이동 : cmd + shift + [ or ]
- 한줄 / 여러줄 주석(토글) : cmd + / , cmd + shift + /
- Find / Replace : cmd + f / cmd + r (엔터로 다음 검색 위치로 이동)
- 선언부 / 사용된 곳으로 이동 : cmd + b
- 선택 확장 / 축소 : opt + up, down
- 행 이동 / 문장 이동 : opt + shift + up/down, cmd + shift + up/down
- 행 복사 / 삭제 : cmd + d / cmd + y
- 자동 들여쓰기 : cmd + alt + i
- 문장 자동 완성 : cmd + shift + enter
- Basic Code Completion : ctrl + space
- insert live template : cmd + j