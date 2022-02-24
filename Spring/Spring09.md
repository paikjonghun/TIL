## 회원 테이블로 게시판 목록 + 등록 기능 만들기

![스크린샷 2022-02-24 오전 10.01.45.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c0a7d73-5f59-4bd9-aa2c-9d6887fa35cf/스크린샷_2022-02-24_오전_10.01.45.png)

- 목록 + 등록 기능 만들기

## ID 중복체크

- DB → M 테이블 편집 → 제약 조건 → 새로운 고유 키 만들고 M_ID 추가

- 컨트롤러 → Writes 주소가 매핑된 메소드에 checkId 메소드 호출해서 개수 받아오기
    
    ```java
    int cnt = iMemberService.checkId(params);
    ```
    

- Service 인터페이스 → Service 클래스 → Dao 인터페이스 → Dao 클래스 메소드 생성 + 오버라이딩

- SQL.XML 에 쿼리 작성 - 테이블에서 M_ID 가 넘어온 id값이랑 겹치는 것의 합계를 넘겨주겠다.
    
    ```java
    <select id="checkId" resultType="Integer" parameterType="hashmap">
    		SELECT COUNT(*) AS CNT
    		FROM M
    		WHERE M_ID = #{id}
    </select>
    ```
    

- 컨트롤러로 돌아와서 if문 작성
    
    ```java
    		if(cnt == 0) {
    			try {
    				iMemberService.memberAdd(params);
    				
    				mav.setViewName("redirect:memberList");
    				
    			} catch(Exception e) {
    				e.printStackTrace();
    				mav.setViewName("test/memberWrites");
    			}
    			
    		} else {
    			mav.addObject("check", "false");
    			mav.setViewName("test/memberWrites");
    		}
    		
    		return mav;
    ```
    

- memberWrites.jsp → 컨트롤러에서 넘어온 check 값 받아서 활용
    
    ```java
    $(document).ready(function() {
    	if('${check}' == "false") {
    		alert("중복된 아이디가 있습니다.");
    		history.back();
    	} else {
    		alert("작성 중 문제가 발생했습니다.");
    	}
    
    });
    ```
    

## 암호화

![스크린샷 2022-02-24 오후 3.14.36.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ccb567b-cb8a-44d5-aa66-74824fb96c81/스크린샷_2022-02-24_오후_3.14.36.png)

- CommonProperties.java
    
    ```java
    	/**
    	 * 암호화키(AES기반 16글자)
    	 */
    	public static final String SECURE_KEY = "goodeesmart12345";
    ```
    

객체 생성 없이 사용 가능하고, 변경이 안 되는 상수인 암호화키. 암호화하고 복호화하는 키.

- Utils.java
    
    ```java
    	/**
    	 * 문자열을 key를 통해 암호화 하고 base64 로 인코딩
    	 * 
    	 * @return String
    	 * @throws Throwable
    	 */
    	public static String encryptAES128(String value) throws Throwable {
    		//
    		// AES 암호화
    		//
    		// AES 암호화 키 생성
    		SecretKeySpec keySpec 
    				= new SecretKeySpec(CommonProperties.SECURE_KEY.getBytes("UTF-8"), "AES");
    		// 암호화 객체(Cipher) 생성(AES 방식으로)
    		Cipher cipher = Cipher.getInstance("AES");
    		// 암호화 모드 설정 및 키 할당
    		cipher.init(Cipher.ENCRYPT_MODE, keySpec);
    		byte[] encrypted = cipher.doFinal(value.getBytes()); // 암호화
    		
    
    		//
    		// Base64 암호화
    		//
    		Encoder encoder = Base64.getEncoder(); // Base64 방식의 암호화 객체 생성
    		
    		String encodeString = encoder.encodeToString(encrypted); // 바이트 타입의 배열을 문자열로 변환
    
    		return encodeString;
    	}
    
    	/**
    	 * key 를 통해 문자열 base64 디코딩
    	 * 
    	 * @return String
    	 * @throws Throwable
    	 */
    	public static String decryptAES128(String value) throws Throwable {
    		// AES 암호화키 생성
    		SecretKeySpec keySpec = new SecretKeySpec(CommonProperties.SECURE_KEY.getBytes("UTF-8"), "AES");
    
    		// 암호화 객체 생성(AES 방식으로)
    		Cipher cipher = Cipher.getInstance("AES");
    		// 암호화 모드 설정 및 키 할당.
    		cipher.init(Cipher.DECRYPT_MODE, keySpec);
    		
    		//
    		// Base64 복호화
    		//
    		Decoder decoder = Base64.getDecoder(); // Base64 복호화 객체 생성
    		
    		byte[] decodeBytes = decoder.decode(value); // 문자열 형태의 파라메터를 배열에 바이트 변환 후 삽입
    		
    
    		//
    		// AES 복호화
    		//
    		byte[] decryptBytes = cipher.doFinal(decodeBytes); // 복호화
    
    		return new String(decryptBytes);
    	}
    ```
    

Utils.javad에 encryptAES128 메소드가 AES암호화 키를 만들어줌. (AES128 - 고급 암호화 표준 기술)

SECURE_KEY를 가지고 암호화, 복호화 가능.

## 로그인

Session : 임시 저장 공간. 사용자의 IP, MAC주소, 브라우저, 서비스에 따라 별도의 공간 제공

Login : Session에 사용자 정보를 보관

같은 정보의 다른 서비스의 경우 Session 공간이 틀림

이를 동기화하기 위해 SSO(Single Sign On) 기술 사용

Logout : Session에 사용자 정보를 제거

## 로그아웃