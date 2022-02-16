## DB와 연동된 게시판 만들기 / 상세보기 페이지 만들기

- TestService에서 @Autowired 만들고 ITestDao 인터페이스 변수 선언

- TestController에서 @Autowired 만들고 ITestService 인터페이스 변수 선언.

- 쿼리가 저장되는 곳은 SQL.xml 이다. Blank_SQL.xml 복사해서 Test_SQL.xml 만들기.
    - `mapper`는 클래스 같은 것. 쿼리 하나하나를 메소드라고 봤을 때, 쿼리를 관리하려고 만들어 놓은 것이 mapper.
    - `namespace` : 클래스명 같은 것.
    - `id` : 메소드명과 동일
    - `resultType` : 취득할 데이터 row의 형태를 지정
    - 쿼리 부분에 ;은 포함되면 안 된다.

- SQL Developer 에서 샘플 테이블 만들기

- 이클립스 TestController에서 주소 연결
    - 화면에 데이터 뿌려줄 것임.
    - 외부에 있는 데이터를 연결할 때에는 무조건적으로 예외처리가 필요함. DB사용시 예외처리 필수
        - 데이터베이스 서버가 죽을 수도 있고, 외부적인 요인으로 오라클과 연결이 안될 때가 있다. 문제가 발생할 수 있다.
        - throws throwable 하는 게 기본
    - Controller → Service → Dao 순서로 외부 데이터를 연결

- Controller에서 iTestService에 getTblist() 메소드 요청
    
    ```java
    	@Autowired
    	public ITestService iTestService;
    
    	@RequestMapping(value = "/tbList")
    	public ModelAndView tbList(ModelAndView mav) throws Throwable {
    		List<HashMap<String, String>> list = iTestService.getTbList();
    		mav.addObject("list", list);
    		mav.setViewName("test/tbList");
    		return mav;
    	}
    
    ```
    

- ITestService에서 getTbList() 메소드 만든다.

- TestSerivce는 ITestService를 참조하고 getTbList() 메소드를 오버라이딩 함.

- ITestDao에 getTbList() 메소드 요청
    
    ```java
    	@Autowired
    	public ITestDao iTestDao;
    	
    	@Override
    	public List<HashMap<String, String>> getTbList() throws Throwable {
    		return iTestDao.getTbList();
    	}
    
    	
    ```
    

- ITestDao에서 getTbList() 메소드 만든다.

- TestDao는 ITestDao를 참조하고 getTbList() 메소드를 오버라이딩 함.
    
    ```java
    @Override
    	public List<HashMap<String, String>> getTbList(HashMap<String, String> params) throws Throwable {
    		// selectList(SQL 위치) : 위치에 있는 SQL을 조회하여 목록을 취득하겠다.
    		// SQL 위치 : namespace.id
    		return sqlSession.selectList("test.getTbList", params);
    	}
    ```
    
    - sqlSession에 쿼리 실행 결과를 요구함. test에 있는 getTbList 쿼리 실행 결과를 넘겨달라.

- .XML 에서 쿼리를 찾기 시작한다. namespace가 test, id가 getTbList 라는 것을 찾아서 결과를 HashMap으로 돌려준다. 하지만 TestDao에서는 리스트로 받기로 되어 있어서 리스트로 받게 됨.

- TestDao → ITestDao → TestService → ITestService → TestController 에서 list로 받게됨.

- 그리고 jsp에 list란 이름으로 넘겨줌



