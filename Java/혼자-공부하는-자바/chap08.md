# ch8. 인터페이스

## 인터페이스

- 핵심 키워드 : 인터페이스, 상수 필드, 추상 메소드, 구현 클래스, 인터페이스 사용
- 핵심 포인트 :
    - 인터페이스란 객체의 사용 방법을 정의한 타입이다.
    - 인터페이스를 통해 다양한 객체를 동일한 사용 방법으로 이용할 수 있다.
    - 인터페이스를 이용해서 다형성을 구현할 수 있다.

- 인터페이스(interface)
    - 개발 코드는 인터페이스를 통해서 객체와 서로 통신한다.
    - 인터페이스의 메소드를 호출하면 객체의 메소드가 호출된다.
    - 개발 코드를 수정하지 않으면서 객체 교환이 가능하다.

### 인터페이스 선언

- 인터페이스 선언
    - ~.java 형태 소스 파일로 작성 및 컴파일러를 통해 ~.class 형태로 컴파일 된다.
    - 클래스와 물리적 파일 형태는 같으나 소스 작성 내용이 다르다.
        - `[public] interface 인터페이스이름 {...}`
    - 인터페이스는 객체로 생성할 수 없으므로 생성자가 가질 수 없다.
    
    ```java
    interface 인터페이스이름 {
    	//상수
    	타입 상수이름 = 값;
    	//추상 메소드
    	타입 메소드이름(매개변수,...)
    }
    ```
    

- 상수 필드(constant field) 선언
    - 데이터를 저장할 인스턴스 혹은 정적 필드 선언 불가
    - 상수 필드만 선언 가능
    
    ```java
    [public static final] 타입 상수이름 = 값;
    ```
    
    - 상수 이름은 대문자로 작성하되 서로 다른 단어로 구성되어 있을 경우 언더바(_)로 연결

- 추상 메소드 선언
    - 인터페이스 통해 호출된 메소드는 최종적으로 객체에서 실행
    - 인터페이스의 메소드는 실행 블록이 필요 없는 추상 메소드로 선언
    - `[public abstract] 리턴타입 메소드이름(매개변수, ... );`
    
- 구현(implement) 클래스
    - 인터페이스에서 정의된 추상 메소드를 재정의해서 실행 내용을 가지고 있는 클래스
    - 클래스 선언부에 implements 키워드 추가하고 인터페이스 이름 명시
    
    ```java
    public class 구현클래스이름 implements 인터페이스이름 {
    	//인터페이스에 선언된 추상 메소드의 실제 메소드 선언
    }
    ```
    

- 인터페이스와 구현 클래스 사용 방법
    - 인터페이스 변수 선언하고 구현 객체를 대입

- 다중 인터페이스 구현 클래스
    - 객체는 다수의 인터페이스 타입으로 사용 가능
    
    ```java
    public class 구현클래스이름 implements 인터페이스A, 인터페이스B {
    	//인터페이스 A에 선언된 추상 메소드의 실체 메소드 선언
    	//인터페이스 B에 선언된 추상 메소드의 실체 메소드 선언
    }
    ```
    

### 인터페이스 사용

- 인터페이스 사용
    - 인터페이스는 필드, 매개 변수, 로컬 변수의 타입으로 선언 가능.
    

### 키워드

- 인터페이스 : 객체의 사용 방법을 정의한 타입. 개발 코드와 객체가 서로 통신하는 접점 역할을 한다. 개발 코드가 인터페이스의 메소드를 호출하면 인터페이스는 객체의 메소드를 호출시킨다. 구성 멤버는 상수 필드와 추상 메소드.
- 상수 필드 : 인터페이스의 필드는 기본적으로 public static final 특성을 가짐 관례적으로 필드 이름은 모두 대문자로 작성. 선언 시 초기값을 대입해야 함.
- 추상 메소드 : 인터페이스의 메소드는 public abstract 생략되고 메소드 선언부만 있는 추상 메소드. 구현 클래스는 반드시 추상 메소드를 재정의해야 한다.
- implements : 구현 클래스에는 어떤 인터페이스로 사용 가능한지 기술하기 위해 사용. 클래스 선언 시 implements 키워드 사용.
- 인터페이스 사용 : 클래스 선언 시 필드, 매개 변수, 로컬 변수로 선언 가능. 구현 객체를 대입.

## 타입 변환과 다형성

핵심 키워드 : 자동 타입 변환. 다형성, 강제 타입 변환, instanceof, 인터페이스 상속

핵심 포인트 : 인터페이스도 메소드 재정의와 타입 변환이 되므로 다형성을 구현할 수 있다.

- 인터페이스의 다형성
    - 인터페이스 사용 방법은 동일하지만 구현 객체 교체하여 프로그램 실행 결과를 다양화
    

### 자동 타입 변환

- 자동 타입 변환(promotion)
    - 구현 객체와 자식 객체는 인터페이스 타입으로 자동 타입 변환 된다.
    

### 필드의 다형성

- 필드의 다형성

### 매개 변수의 다형성

- 매개변수의 다형성

### 강제 타입 변환

- 강제 타입 변환(casting)
    - 구현 객체가 인터페이스 타입으로 자동 변환하면 인터페이스에 선언된 메소드만 사용 가능
    - 구현 클래스에만 선언된 필드나 메소드를 사용할 경우 강제 타입 변환

### 객체 타입 확인

- 객체 타입 확인 instanceof
    - 구현 객체가 변환되어 있는지 알 수 없는 상태에서 강제 타입 변환할 경우 ClassCastException qㅏㄹ생.
    - instanceof 연산자로 확인 후 안전하게 강제 타입 변환

- 인터페이스 상속
    - 인터페이스는 다중 상속을 할 수 있다.