## AOP(Aspect Oriented Programming) - 관점 지향 프로그래밍

- 프로그램의 실행을 각 시점으로 분리하여 기능을 추가하는 프로그래밍 방식
- Advice - 실행 관점(Before, After, Around, After-Returning, After-Throwing)
- Pointcut - 관점을 적용시킬 대상 범위
- JoinPoint - 이벤트 대상

- CommonAOP.java
    - @Aspect - 관점 지향 설정 객체

- pom.xml
    - `AspectJ` - aspectjweaver, aspectjtools 두 개를 추가로 가져와야 함.

## AOP 방식으로 로그인

- TestAMController.java
    - 메소드 하나만 변경할 때는 그냥 변경하는 게 낫다.
    
    ```jsx
    	@RequestMapping(value = "/amLogin")
    	public ModelAndView amLogin(HttpSession session, ModelAndView mav) {
    		if(session.getAttribute("sMNo") != null) {
    			mav.setViewName("redirect:atbList");
    		} else {
    			mav.setViewName("testa/amLogin");
    		}
    		return mav;
    	}
    ```
    

- ATBController.java
    
    로그인 된 사용자만 등록, 수정할 수 있기 때문에 AOP를 활용해 한번에 처리하기
    
    ```jsx
    
    ```
    

- CommonAOP.java
    
    ```jsx
    	@Pointcut("execution(* com.spring.sample..ATBController.atbWrite(..))" 
    			+ "|| execution(* com.spring.sample..ATBController.atbUpdate(..))")
    	public void testAOP() {}
    ```
    

- 2 Spring AOP marker at this line
    
    ![스크린샷 2022-03-08 오전 11.49.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b4069da-80bb-4011-88e1-53e27f828f9f/스크린샷_2022-03-08_오전_11.49.29.png)
    

- ATBController.java에서 AOP가 잘 걸린 게 보임
    
    ![스크린샷 2022-03-08 오전 11.57.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfd4428d-fa0d-4a67-8b01-4ba90f81d21e/스크린샷_2022-03-08_오전_11.57.53.png)
    
    ![스크린샷 2022-03-08 오전 11.57.43.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee538fb9-0508-4920-a787-dba9937be355/스크린샷_2022-03-08_오전_11.57.43.png)
    

- atbList에서 등록 페이지 또는 atb의 수정 페이지로 가면 aop 실행

- 로그인 권한이 필요한 곳에 전부 세션처리를 하면 너무 길어짐. 세션에 관련해서 변경이 생기거나 기능이 추가되면 일일이 다 수정을 해야함. 그러면 작업들이 너무 많아짐. 그렇기 때문에 한번에 처리한 것임. 보통은 로그인이나 권한 체크할 때 많이 씀.

- 주의사항으로는 ajax에 AOP 태우고 싶으면, ajax용 AOP를 따로 만들어야함.

- servlet-context.xml
    
    ```jsx
    <!-- AOP -->
    <aop:aspectj-autoproxy />
    <beans:bean id="commonAop" class="com.spring.sample.common.controller.CommonAOP" />
    ```
    
    - Spring AOP를 활성화 하겠다. 객체를 만들겠다.
    
- 하지만, 일일이 추가하기 귀찮기 때문에 @Component를 추가하면 객체 만들어짐.
    
    ```jsx
    // @Component - 말 그대로 객체. 용도가 존재하지 않는 객체를 나타낼 때 사용.
    @Component
    // @Aspect - 관점 지향 설정 객체
    @Aspect
    public class CommonAOP {
    ```
    

## 스케줄링

- Batch - 주기적으로 실행되는 프로그램

- Java - 스케줄링
- Thread : 일정 간격으로 자동 실행
- Quartz : 스케줄링 라이브러리

- Spring - 스케줄링
- Spring Scheduler

- pom.xml
    - pom.xml - aop에 대한 설정도 들어가 있다.
    - task - 작업
    
- BatchComponent.java
    - 객체를 생성해야하기 때문에 컴포넌트를 만들어 놓고
    - @Scheduled를 추가하면 됨. cron은 시간 설정
    - 

- servlet-context.xml
    
    ```jsx
    	<!-- Spring scheduler -->
    	<task:scheduler id="scheduler" pool-size="50"/>
    	<task:annotation-driven scheduler="scheduler"/>
    ```
    

# 에러 페이지

- web.xml
    
    ```xml
    	<error-page>
    		<error-code>404</error-code>
    		<location>/WEB-INF/views/exception/PAGE_NOT_FOUND.jsp</location>
    	</error-page>
    	<error-page>
    		<error-code>405</error-code>
    		<location>/WEB-INF/views/exception/NOT_ALLOW_ACCESS.jsp</location>
    	</error-page>
    ```
    
    - http error 404(not found) : 주소나 화면이 존재하지 않을 때. 서버가 요청한 페이지를 찾을 수 없다.
    - http error 405 : 요청에 지정된 방법을 사용할 수 없다.
    - http error 500(내부 서버 오류) : 서버에 오류가 발생하여 요청을 수행할 수 없다.
    - http error 505 :

- servlet-context - *exceptionMapping*
    - 예외 발생시 EXEPTION_INFO 페이지에 데이터 전송
    
    ```xml
    <!-- Exception Resolver -->
    	<beans:bean id="exceptionMapping" 	class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    		<beans:property name="exceptionMappings">
    			<beans:props>
    				<beans:prop key="java.lang.Exception">/exception/EXCEPTION_INFO</beans:prop>
    				<beans:prop 
    					key="com.spring.sample.exception.UserNotFoundException">
    					/exception/USER_NOT_FOUND_EXCEPTION
    				</beans:prop>
    			</beans:props>
    		</beans:property>
    	</beans:bean>
    ```
    
- multipartResolver - 파일 업로드와 관련된 것. value는 바이트 단위. 현재는 약 9MB.
    
    ```xml
    <!-- multipartResolver -->
    	<beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    		<beans:property name="maxUploadSize" value="10000000" />
    	</beans:bean>
    ```