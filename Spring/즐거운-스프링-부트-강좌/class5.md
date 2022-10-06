# Web Application

- 웹 애플리케이션(web application) 또는 웹 앱은 소프트웨어 공학적 관점에서 인터넷이나 인트라넷을 통해 웹 브라우저에서 이용할 수 있는 응용 소프트웨어를 말한다.

# Web Application Server

- 웹 애플리케이션 서버(Web Application Server, 약자 WAS)는 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 프레임워크다. 인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해 주는 미들웨어(소프트웨어 엔진)로 볼 수 있다. 웹 애플리케이션 서버는 동적 서버 콘텐츠를 수행하는 것으로 일반적인 웹 서버와 구별이 되며, 주로 데이터베이스 서버와 같이 수행이 된다. 한국에서는 일반적으로 “WAS” 또는 “WAS S/W”로 통칭하고 있으며 공공기관에서는 “웹 응용 서버”로 사용되고, 영어권에서는 “Application Server”(약자 AS)로 불린다.
- 자바 EE 표준 준수 웹 애플리케이션 서버
    - 스프링, 스프링 부트를 사용하는 사용자는 이것을 WAS라고 말한다.
    - 자바 EE 스펙을 지키며 만드는 것
    - 자바 기반이나 자바 EE 비준수 웹 애플리케이션 서버
        - 대표적인 것이 아파치 톰캣

# Java EE에 대한 표준을 일부 준수

- 아파치 톰캣(Apache Tomcat) : 오픈 소스 재단 아파치 소프트웨어 재단의 오픈 소스 소프트웨어

# Java EE Platform Specification

## Java EE에서 Jakarta EE로의 전환

- 기존에는 Java EE 용어를 썼지만, 최신 웹 애플리케이션 서버는 Jakarta EE 용어로 바뀌었다.

# Java 웹 프로그래밍

- Java 웹 프로그래밍이란? Java 언어로 웹 어플리케이션을 만들겠다.
- 웹 어플리케이션이 실행될 수 있는 WAS가 필요하다.
    - ex) iPhone앱은 IOS 위에서 동작한다.
- Servlet/JSP를 실행할 수 있는 환경(Servlet 컨테이너)
    - JSP도 알고보면 Servlet 기술로 만들어진다.
    - Servlet 컨테이너는 WAS 안에 있는 것.
    - WAS는 여러 개의 웹 어플리케이션을 실행할 수 있다.
    - 대표적인 WAS는 Tomcat

# Tomcat?

- https://tomcat.apache.org
    - apache-tomcat-8.5.82.tar.gz을 다운 받는다.
    - 특정 폴더에 복사한 후 압축을 해제
        - tar xvfz apache-tomcat-8.5.82.tar.gz
    - apache-tomcat-8.5.82/bin
        - startup.sh을 실행하면 서버가 실행된다.
            - 윈도우는 startup.bat 파일을 실행한다.
        - shutdown.sh을 실행하면 서버가 종료된다.
            - 윈도우는 shutdown.bat 파일을 실행한다.

# startup.sh을 수정한 후 실행한다.

- 마지막 줄
    - `exec "$PRGDIR"/"$EXECUTABLE" start "$@"`
- 다음과 같이 수정한다.
    - `exec "$PRGDIR"/"$EXECUTABLE" run "$@"`
- 그러면 백그라운드가 아니라 포그라운드로 실행된다.

# Tomcat이 기본으로 제공하는 Web Application

- apache-tomcat-8.5.82/webapps

# Tomcat이 성공적으로 실행되었다면,

- http://localhost:8080/
    - ROOT 웹 어플리케이션
- http://localhost:8080/docs/
    - docs 웹 어플리케이션
- http://localhost:8080/exaples/
    - examples 웹 어플리케이션

# exaples 폴더의 구조

```xml
paikjonghun@baegjonghun-ui-MacBookPro examples % ls -la
total 8
drwxr-x---@  8 paikjonghun  staff   256 12  2  2021 .
drwxr-x---@ 12 paikjonghun  staff   384  8 10 21:33 ..
drwxr-x---@  3 paikjonghun  staff    96 12  2  2021 META-INF
drwxr-x---@  8 paikjonghun  staff   256 12  2  2021 WEB-INF
-rw-r-----@  1 paikjonghun  staff  1126 12  2  2021 index.html
drwxr-x---@ 22 paikjonghun  staff   704 12  2  2021 jsp
drwxr-x---@ 11 paikjonghun  staff   352 12  2  2021 servlets
drwxr-x---@  7 paikjonghun  staff   224 12  2  2021 websocket
```

# Tomcat을 이용한 웹 어플리케이션을 만든다는 의미는?

- [http://localhost:8080/](http://localhost:8080/) 요청을 했을 때,
    - 내가 만든 사이트가 보여지려면?
        - webapps/ROOT의 내용을 내가 만든 내용으로 바꾸면 된다.
        - Tomcat 서버에 내가 만든 웹 어플리케이션을 deploy(배치, 배포)한다. 라고 표현.

# 서버와 브라우저의 동작

- Tomcat 서버가 OS 위에서 실행 된다.
- 포트 값은 기본으로 8080으로 실행된다.
- 운영체제 안에는 여러 개의 서버가 실행될 수 있다. 단, 각각의 포트값이 달라야 한다.
- 다른 컴퓨터에 있는 브라우저로 접속하기 위해서는 컴퓨터의 IP 주소와 포트값을 알아야 한다.
    1. HTTP 프로토콜이 연결되어야 함
    2. 요청 정보를 보냄(URL PATH)
- 브라우저가 같은 컴퓨터 안에 있다고 한다면, 아래처럼 할 수 있다.
    - ip주소 = 127.0.0.1
    - domain = [localhost](http://localhost)

# 웹 어플리케이션을 어떻게 만들까?

- Eclipse IDE, IntelliJ IDE는 손쉽게 가능하게 해준다.

# IntelliJ에서 웹 어플리케이션 만들기

- 프로젝트 이름 : servletexam
- 실행 : [http://localhost:8080/servletexam_war_exploded/](http://localhost:8080/servletexam_war_exploded/)
- 내가 만든 웹 어플리케이션은 ROOT로 동작하지 않는다.
- [servletexam_war_exploded](http://localhost:8080/servletexam_war_exploded/) 이름의 웹 어플리케이션이 등록되었음.
- src/main/webapp/index.jsp 가 보여진 것.

# [http://localhost:8080/](http://localhost:8080/) 으로 실행하고 싶다면?

- Tomcat configuration 에서 Application Context를 ‘/’ 로 바꿔준다.