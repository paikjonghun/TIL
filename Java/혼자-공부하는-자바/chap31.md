## 56강 - java.lang 패키지

- 핵심 키워드 : Object 클래스, System 클래스, Class 클래스, String 클래스, Wrapper 클래스, Math 클래스
- 핵심 포인트 :
    - java.lang 패키지는 자바 패키지는 자바 프로그램의 기본적인 클래스를 담은 패키지다.
    - java.lang 패키지의 클래스와 인터페이스는 import 없이 사용할 수 있다.

- Object 클래스 - 자바 클래스의 최상위 클래스로 사용
- System 클래스 - 표준 입출력장치 이용, 자바 가상 기계 종료할 때, 쓰레기 수집기를 실행 요청할 때 사용
- Class 클래스 - 클래스를 메모리로 로딩할 때 사용
- String 클래스 - 문자열을 저장하고 여러가지 정보를 얻을 때 사용
- Wrapper 클래스 - 포장 클래스.
    - Byte, Short, Chracter, Integer, Float, Double, Boolean, Long 등 기본 타입 데이터를 갖는 객체 만들 때 사용.
    - 입력값 검사에 사용
- Math 클래스 - 수학 함수를 사용할 때 사용.

- 자바 API(Aplication Programming Interface)
    - 프로그램 개발에 자주 사용되는 자바에서 제공하는 클래스 및 인터페이스 모음
    - 자바 라이브러리라고도 함
    - API 도큐먼트를 통해 사용 방법을 확인할 수 있다.

- API 도큐먼트에서 클래스 페이지 읽는 방법
    - 최상단 SUMMARY : NESTED | FIELD | CONSTR | METHOD
        - SUMMARY : 클래스 내 선언된 멤버가 어떤 것들이 있는지 알려줌
        - NESTED : 중첩 클래스 혹은 인터페이스 여부
    - 클래스 선언부
        - final 혹은 abstract 키워드 있는지 확인
        - extends 뒤 언급된 부모 클래스 확인
        - implements 키워드 뒤의 인터페이스 확인
    - 클래스에 선언된 필드 목록
        - SUMMARY : NESTED | FIELD | CONSTR | METHOD 에서 FIELD 클릭하여 필드 목록으로 이동
    - 클래스에 선언된 생성자 목록
        - SUMMARY 에서 CONSTR 클릭하여 생성자 목록 이동
        - 생성자 이름 클릭하면 상세 페이지로 이동
    - 클래스에 선언된 메소드 목록
        - SUMMARY 에서 METHOD 클릭하여 메소드 목록으로 이동
    

## Object 클래스

- 모든 클래스는 Object 클래스의 자식이거나 자손 클래스
    - System 클래스, String 클래스, Number 클래스, Integer 클래스 등

- 객체 비교(equals())
    - equals() 메소드의 매개 타입은 Object로, 모든 객체가 매개값으로 대입될 수 있음
    - Object 클래스의 equals() 메소드는 비교 연산자인 ==와 동일 결과 리턴
    
    ```jsx
    Object obj1 = new Object();
    Object ojb2 = new Object();
    
    boolean result = obj1.equals(obj2);
    
    boolean result = (obj1 == obj2)
    ```
    

- equals() 메소드 재정의하는 경우
    - 두 객체가 필드값이 같은 동등 객체인지 비교하기 위해
    - 두 객체의 필드가 모두 같으면 true를, 그렇지 않으면 false 리턴
    - equals() 메소드는 매개값이 기준 객체와 동일 타입 객체인지 먼저 확인 필요

- 객체 해시코드 (hashCode())
    - 객체를 식별하는 하나의 정수값
    - Object 클래스의 객체 해시코드 메소드는 객체 메모리 번지 이용하여 만듦 → 객체마다 다른 값
    - hashCode를 재정의하는 경우 :
        - 두 객체가 동등한지 비교할 때 필요
    
    ```java
    public class KeyExample {
    	public static void main(String[] args) {
    		HashMap<String, String> hashMap = new HashMap<String, String>();
    	}
    }
    ```
    
- 객체 문자 정보 (toString())
    - Object 클래스의 toString() 메소드는 객체의 문자 정보 리턴
        - 기본적으로 ‘클래스이름@16진수해시코드’로 구성된 문자 정보
    - toString() 메소드를 재정의하는 경우
        - 유익한 정보 리턴하기 위해
    

## System 클래스

- System 클래스의 모든 필드와 메소드는 정적 필드 및 메소드로 구성
- 프로그램 종료 (exit())
    - exit() 메소드 호출하여 JVM을 강제 종료
    - exit() 메소드가 지정하는 int 매개값을 종료 상태값이라 함
- 현재 시각 읽기 (currentTimeMillis(), nanoTime())
    - currentTimeMillis() : 1/10의 3승 단위 long 값 리턴
    - nanoTime() : 1/10의 9승 단위 long 값 리턴

## Class 클래스

- 자바는 클래스와 인터페이스의 메타 데이터를 Class 클래스로 관리
    - 메타 데이터 : 타입 이름 및 파일 경로 정보, 필드 정보, 생성자 정보, 메소드 정보
- Class 객체 얻기 (getClass(), forName())
    - 클래스로부터 얻는 방법
        - Class clazz = 클래스이름.class
        - Class clazz = Class.forName(”패키지...클래스이름”)
    - 객체로부터 얻는 방법
        - Class clazz = 참조변수.getClass();
    - ex) String 클래스
        - Class clazz = String.class;
        - Class clazz = Class.forName(”java.lang.string”);
        - String str = “감자바”;
        - Class clazz = str.getClass();
- 클래스 경로 활용하여 리소스 절대 경로 얻기
    - Class 객체는 파일 경로 정보 가지고 있어 이 경로 활용해 다른 리소스 파일의 경로를 얻을 수 있다.

## String 클래스

- String 생성자
    - 직접 String 객체를 생성
    
    ```java
    // 배열 전체를 String 객체로 생성
    String str = new String(byte[] bytes);
    // 지정한 문자셋으로 디코딩
    String str = new String(byte[] bytes, String charsetName);
    
    // 배열의 offset 인덱스 위치부터 length만큼 String 객체로 생성
    String str = new String(byte[] bytes, int offset, int length);
    // 지정한 문자셋으로 디코딩
    String str = new String(byte[] bytes, int offset, int length, String charsetName)
    
    ```
    

- String 메소드
    - `charAt(int index)` : 특정 위치의 문자를 리턴
    - `equals(Object anObject)` : 두 문자열을 비교
    - `getBytes()` : byte()로 리턴
    - `getBytes(Charset charset)` : 주어진 문자셋으로 인코딩한 byte()로 리턴
    - `indexOf(String str)` : 문자열 내에서 주어진 문자열의 위치를 리턴
    - `length()` : 총 문자의 수를 리턴
    - `replace(CharSequence target, CharSequence replacement)` : target 부분을 replacement로 대치한 새로운 문자열을 리턴.
    - `substring(int beginindex)` : beginindex 위치에서 끝까지 잘라낸 새로운 문자열을 리턴합니다.
    - `substring(int beginindex, int endindex)` : beginindex 위치에서 endindex 전까지 잘라낸 새로운 문자열을 리턴
    - `toLowerCase()` : 알파벳 소문자로 변환한 새로운 문자열을 리턴
    - `toUpperCase()` : 알파벳 대문자로 변환한 새로운 문자열을 리턴
    - `trim()` : 앞뒤 공백을 제거한 새로운 문자열을 리턴
    - `valueOf(int i)` : 기본 타입 값을 문자열로 리턴
    - `valueOf(double d)` : 기본 타입 값을 문자열로 리턴
    
- 문자 추출(`charAt()`)
    - 매개값으로 주어진 인덱스의 문자를 리턴
- 문자열 비교(`equals()`)
    - == 연산자 사용할 경우 원하지 않는 결과 나올 수 있음
- 바이트 배열로 변환(`getBytes()`)
    - getBytes()의 메소드는 시스템의 기본 문자셋으로 인코딩된 바이트 배열 리턴
    - getBytes(Charset charset) 메소드는 특정 문자셋으로 인코딩된 바이트 배열 리턴
    - 바이트 배열을 다시 문자열로 디코딩할 때에는 어떤 문자셋으로 인코딩되었는가에 따라 다름
- 문자열 찾기(`indexOf()`)
    - 매개값으로 주어진 문자열이 시작되는 인덱스 리턴
    - 주어진 문자열 포함되어 있지 않으면 -1 리턴
- 문자열 길이(`length()`)
    - 문자열의 길이를 리턴
- 문자열 대치(`replace()`)
    - 첫 번째 매개값인 문자열을 찾아 두 번째 매개값인 문자열로 대치한 새로운 문자열 생성 및 리턴
- 문자열 잘라내기(`substring()`)
    - 주어진 인덱스에서 문자열을 추출
    - substring(int beginindex, int endindex)는 주어진 시작과 끝 인덱스 사이의 문자열 추출
    - substring(int beginindex)는 주어진 인덱스부터 끝까지 문자열 추출
- 알파벳 소, 대문자 변경(`toLowerCase()`, `toUpperCase()`)
- 문자열 앞뒤 공백 잘라내기(`trim()`)
    - 문자열의 앞뒤 공백 제거한 새로운 문자열 생성 및 리턴
- 문자열 변환(`valueOf()`)
    - 기본 타입의 값을 문자열로 변환

## Wrapper(포장) 클래스

- 포장 객체
    - 기본 타입의 값을 내부에 두고 포장
    - 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없음
    - byte, char, short, int, long, float, double, boolean 기본 타입 값 갖는 객체
- Boxing과 Unboxing
    - 박싱 : 기본 타입의 값을 포장 객체로 만드는 과정
    - 언박싱 : 포장 객체에서 기본 타입의 값을 얻어내는 과정
- 생성자 이용하지 않고 각 포장 클래스마다 가진 정적 valueOf() 메소드 활용
- `기본 타입 이름 + value()` 메소드 호출하여 언박싱
- 자동 박싱과 언박싱
    - 포장 클래스 타입에 기본값이 대입될 경우 자동 박싱 발생
        
        ```java
        Integer obj = 100; // 자동 박싱
        ```
        
    - 기본 타입에 포장 객체가 대입되는 경우 및 연산에서 자동 언박싱 발생
    
    ```java
    Integer obj = new Integer(200);
    int value1 = obj; // 자동 언박싱
    int value2 = obj + 100; // 자동 언박싱
    ```
    

- 문자열을 기본 타입 값으로 변환
    - 포장 클래스로 문자열을 기본 타입 값으로 변환
    - ‘parse + 기본 타입 이름' 정적 메소드
        - byte : `Byte.parseByte(”10”);`
        - short : `Short.parseShort(”100”);`
        - int : `Integer.parseInt(”1000”);`
        - long : `Long.parseLong(”10000”);`
        - float : `Float.parseFloat(”2.5F”);`
        - double : `Double.parseDouble(”3.5”);`
        - boolean : `Boolean.parseBoolean(”true”);`
    
- 포장 값 비교
    - 포장 객체는 내부 값 비교하기 위해 == 및 != 연산자 사용하지 않는 것이 좋음
    - 연산 결과는 false
        
        ```java
        Integer Obj 1 = 300;
        Integer Obj 2 = 300;
        System.out.println(obj1 == ojb2);
        ```
        
    - 박싱된 값이 아래 표에 나와 있는 범위의 값이 아닌 경우 언박싱한 값을 얻어 비교해야 함. 범위 안에 있는 값이면 같은 객체를 참조하기 때문에 비교 가능. 그렇지만 값 범위에 따라 다르기 때문에 equals 메소드를 사용하는 것이 좋다.
        - boolean : true, false
        - char : \u0000 ~ \u007f
        - byte, short, int : -128 ~ 127

## Math 클래스

- Math 클래스
    - 수학 계산에 사용
        - int abs(int a) 메소드 : 절대값
        - double abs(double a) 메소드 : 절대값
        - double ceil(double a) 메소드 : 올림값
        - double floor(double a) 메소드 : 버림값
        - int max(int a, int b) 메소드 : 최대값
        - double max(double a, double b) 메소드 : 최대값
        - int min(int a, int b) 메소드 : 최소값
        - double min(double a, double b) 메소드 : 최소값
        - double random() : 랜덤값
        - double rint(double a) : 가까운 정수의 실수 값 - 0.0을 포함하고 1.0을 포함하지 않는 랜덤 실수를 리턴
        - long round(double a) : 반올림값
        
- Math.random() 메소드는 0.0 이상 1.0 미만 범위에 속하는 하나의 double 타입 값 리턴
    - 1부터 10까지의 임의의 정수 난수 얻기
        
        ```java
        int num = (int)(Math.random() * 10) + 1
        int num2 = Math.floor(Math.random() * 10) + 1
        ```
        
    
- 핵심 포인트
    - Object 클래스 : 자바의 최상위 부모 클래스, Object 클래스의 메소드는 모든 자바 객체에서 사용 가능
    - System 클래스 : 운영체제의 일부 기능 이용할 수 있음. 정적 필드와 정적 메소드로 구성
    - Class 클래스 : 클래스와 인터페이스의 메타 데이터가 Class 클래스로 관리됨.
    - String 클래스 : String 클래스의 다양한 생성자 이용하여 직접 String 객체를 생성 가능
    - Wrapper 클래스 : 기본 타입의 값 갖는 객체를 포장 객체라 함. 기본 타입의 값을 포장 객체로 만드는 것을 박싱, 반대의 과정을 언박싱이라 함.
    - Math 클래스 : 수학 계싼에 사용할 수 있는 메소드 제공하며, Math 클래스가 제공하는 메소드는 모두 정적 메소드이므로 Math 클래스로 바로 사용 가능.