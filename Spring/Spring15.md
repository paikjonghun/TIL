## AOP(Aspect Oriented Programming) - 관점 지향 프로그래밍

![스크린샷 2022-03-08 오전 10.54.27.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efce2ce5-5435-47e6-8575-bfce4fc1b5dc/스크린샷_2022-03-08_오전_10.54.27.png)

- 프로그램의 실행을 각 시점으로 분리하여 기능을 추가하는 프로그래밍 방식
- Advice - 실행 관점(Before, After, Around, After-Returning, After-Throwing)
- Pointcut - 관점을 적용시킬 대상 범위
- JoinPoint - 이벤트 대상

- CommonAOP.java
    - `@Aspect` - 관점 지향 설정 객체
    
    ```java
    @Aspect
    public class CommonAOP {
    	//Pointcut -> 적용범위
    	//@Pointcut(범위설정)
    	/*
    	 * 범위
    	 * execution -> include필터
    	 * !execution -> exclude필터
    	 * * -> 모든것
    	 * *(..) -> 모든 메소드
    	 * .. -> 모든 경로
    	 * && -> 필터 추가
    	 * || -> 필터 추가
    	 */
    	// CalendarController 안에 모든 메소드를 적용 범위 설정 - 그룹화 해서 testAOP()라는 이름을 붙여놓은 것.
    	@Pointcut("execution(* com.spring.sample..CalendarController.*(..))")
    	public void testAOP() {}
    
    	//ProceedingJoinPoint -> 대상 적용 이벤트 필터
    	/*
    	 * @Before -> 메소드 실행 전
    	 * @After -> 메소드 실행 후
    	 * @After-returning -> 메소드 정상실행 후
    	 * @After-throwing -> 메소드 예외 발생 후
    	 * @Around -> 모든 동작시점
    	 */
    	// 이 메소드가 지정하고 있는 것들의 모든 동작시점(Around)에서 동작하기
    	// ProceedingJoinPoint 이벤트 발생 대상. this랑 같다!
    	@Around("testAOP()")
    	public ModelAndView testAOP(ProceedingJoinPoint joinPoint)
    														throws Throwable {
    		ModelAndView mav = new ModelAndView();
    		
    		// Request 객체 취득
    		HttpServletRequest request
    		= ((ServletRequestAttributes)RequestContextHolder.currentRequestAttributes()).getRequest();
    		
    		System.out.println("------- testAOP 실행됨 ------");
    		
    		return mav;
    	}
    ```
    

- pom.xml
    - `AspectJ` - aspectjweaver, aspectjtools 두 개를 추가로 가져와야 함.
    
    ```xml
    		<!-- AspectJ -->
    		<dependency>
    			<groupId>org.aspectj</groupId>
    			<artifactId>aspectjrt</artifactId>
    			<version>${org.aspectj-version}</version>
    		</dependency>
    		
    		<dependency>
    			<groupId>org.aspectj</groupId>
    			<artifactId>aspectjweaver</artifactId>
    			<version>${org.aspectj-version}</version>
    		</dependency>
    		
    		<dependency>
    			<groupId>org.aspectj</groupId>
    			<artifactId>aspectjtools</artifactId>
    			<version>${org.aspectj-version}</version>
    		</dependency>
    ```
    

## AOP 방식으로 로그인

- TestAMController.java
    - 메소드 하나만 변경할 때는 그냥 직접 변경하는 게 낫다.
    
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
    

- 로그인된 사용자만 등록, 수정할 수 있기 때문에 AOP를 활용해 로그인된 사용자만 atbWrite와 atbUpdate에 들어갈 수 있고, 그렇지 않으면(로그인 없이 주소창으로 이동하면) 로그인 화면으로 이동하도록 처리하기

- CommonAOP.java
    
    ```jsx
    	@Pointcut("execution(* com.spring.sample..ATBController.atbWrite(..))" 
    			+ "|| execution(* com.spring.sample..ATBController.atbUpdate(..))")
    	public void testAOP() {}
    ```
    

- 2 Spring AOP marker at this line. 2개의 메소드가 AOP 걸림
    
    ![스크린샷 2022-03-08 오전 11.49.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b4069da-80bb-4011-88e1-53e27f828f9f/스크린샷_2022-03-08_오전_11.49.29.png)
    

- ATBController.java에서 AOP가 잘 걸린 게 보임
    
    ![스크린샷 2022-03-08 오전 11.57.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfd4428d-fa0d-4a67-8b01-4ba90f81d21e/스크린샷_2022-03-08_오전_11.57.53.png)
    
    ![스크린샷 2022-03-08 오전 11.57.43.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee538fb9-0508-4920-a787-dba9937be355/스크린샷_2022-03-08_오전_11.57.43.png)
    

- atbList에서 등록 페이지 또는 atb의 수정 페이지로 가면 aop 실행됨. 그리고 주소창에 atbWrite나 atbUpdate 입력해도 aop 실행됨.

- 로그인 권한이 필요한 곳에 전부 세션처리를 하면 너무 길어짐. 세션에 관련해서 변경이 생기거나 기능이 추가되면 일일이 다 수정을 해야함. 그러면 작업들이 너무 많아짐. 그렇기 때문에 한번에 처리한 것임. 보통은 로그인이나 권한 체크할 때 많이 씀.

- 주의사항으로는 ajax에 AOP 태우고 싶으면, ajax용 AOP를 따로 만들어야함.

- servlet-context.xml
    
    ```jsx
    <!-- AOP -->
    <aop:aspectj-autoproxy />
    <beans:bean id="commonAop" class="com.spring.sample.common.controller.CommonAOP" />
    ```
    
    - Spring AOP를 활성화 하겠다. 객체를 만들겠다.
    
- 하지만, 일일이 추가하기 귀찮기 때문에 servlet-context.xml에 bean 추가할 필요 없이 @Component를 추가하면 객체 만들어짐.
    
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
    - 객체를 생성해야하기 때문에 @Component 달기
    - @Scheduled를 추가하면 됨. cron은 시간 설정을 뜻함
    
    ```java
    	@Component
    	public class BatchComponent {
    
    	// 초 분 시 일 월 요일
    	// * - 모든
    	// ? - 월, 요일에 사용. 신경안씀이라는 의미
    	// 월은 1 - 12
    	// 요일은 1(일) - 7(토). ,(컴마)로 복수지정가능. 예)월수금 - 2,4,6
    	// 숫자1/숫자2의 경우 숫자1에서 시작하여 매 숫자2마다 실행. 예) 분에 0/5이면 0분부터 5분마다 실행
    	// 일에서 L은 달의 마지막날. W는 지정일자가 휴일(토, 일)이면 인접한 평일에 수행.
    	// 예) 25W인경우 25일이 일요일이면 26일 월요일 실행.
    	// LW는 마지막 평일
    	// 요일에서 숫자1#숫자2의경우 숫자2번째 주의 숫자1번 요일에 실행.
    	// 예) 2#4 - 4번째주 월요일에 실행.
    	@Scheduled(cron = "0 0 0 * * *")
    	public void cronTest1() {
    		System.out.println("batch!!");
    	}
    ```
    

- servlet-context.xml
    - pool-size : 배치 개수 설정
    
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

- servlet-context - Exception Resolver
    - 예외 발생시 EXEPTION_INFO 페이지에 예외 데이터 전송
    
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
    
- servlet-context - multipartResolver - 파일 업로드와 관련된 것. value는 바이트 단위. 현재는 약 9MB.
    
    ```xml
    <!-- multipartResolver -->
    	<beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    		<beans:property name="maxUploadSize" value="10000000" />
    	</beans:bean>
    ```