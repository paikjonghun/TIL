# 01. 원격 프로그램의 실행

| 패스트캠퍼스 - 스프링의 정석 강의를 보며 배운 것들 정리

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