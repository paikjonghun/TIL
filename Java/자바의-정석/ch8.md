## 8-1. 프로그램 오류

- 컴파일 에러(compile-time error) : 컴파일 할 때 발생하는 에러
    - 자바 컴파일러가 하는 일
        1. 구문 체크
        2. 번역
        3. 소스 코드의 최적화 - 상수끼리의 계산정도는 컴파일러가 할 수 있다. / 생략된 코드 추가.
- 런타임 에러(runtime error) : 실행할 때 발생하는 에러
    - Java의 런타임 에러
        - 에러(error) - 프로그램 코드에 의해서 수습될 수 없는 **심각한 오류**
        - 예외(exception) - 프로그램 코드에 의해서 수습될 수 있는 다소 **미약한 오류**
- 논리적 에러(logical error) : 작성 의도와 다르게 동작

- 에러(error)는 어쩔 수 없지만, 예외(exceoption)는 처리하자.
- 예외 처리(exception handling)의 정의와 목적
    - 정의 - 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
    - 목적 - 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것.

## 8-2. 예외 클래스의 계층 구조

- Exception 클래스와 자손들 - 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
- RuntimeException 클래스와 자손들 - 프로그래머의 실수로 발생하는 예외

## 8-4. 예외 처리하기. try-catch 문.

```java
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception2 e2) {
	// Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (ExceptionN eN) {
	// ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

- if문과 달리, try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

- try 블럭 내에서 예외가 발생한 경우
    1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
    2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch 문을 빠져나가서 그 다음 문장을 계속해서 수행한다. 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다. 예외가 발생한 이후의 try 블럭 안의 문장은 실행되지 않는다.
- try 블럭 내에서 예외가 발생하지 않은 경우,
    1. catch 블럭을 거치지 않고 전체 try-catch 문을 빠져나가서 수행을 계속한다.

- 예외가 발생하면, 이를 처리할 catch블럭을 찾아 내려감.
- 일치하는 catch블럭이 없으면, 예외는 처리 안됨.
- Exception이 선언된 catch 블럭은 모든 예외 처리(마지막 catch 블럭)

## 8-7. printStackTrace()와 getMessage()

- printStackTrace() - 예외 발생 당시의 호출 스택(call stack)에 있었던 메소드의 정보와 예외 메시지를 화면에 출력한다.
- getMessage() - 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

## 8-8. 멀티 catch블럭

- 내용이 같은 catch 블럭을 하나로 합친 것(JDK1.7부터 가능)
- 부모자식 관계의 예외일 땐 부모타입의 참조변수가 써져있는 catch 블럭만 작성하면 됨.

## 8-9. 예외 발생시키기

1. 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
    
    ```java
    Exception e = new Exception("고의로 발생시켰음");
    ```
    
2. 키워드 throw를 이용해서 예외를 발생시킨다.
    
    ```java
    throw e;
    ```
    

## 8-10. checked예외, unchecked예외

- checked예외 : 컴파일러가 예외 처리 여부를 체크(예외 처리 필수 - try-catch 필수)
    - Exception 클래스와 그 자손들 - checked 예외 - 예외 처리 필수 - 예외처리 안하면 컴파일 에러
- unchecked에외 : 컴파일러가 예외 처리 여부를 체크 안함(예외 처리 선택 - try-catch 선택)
    - RuntimeException 클래스와 그 자손들 - unchecked 예외 - 예외 처리 선택 - 예외처리 안해도 컴파일은 됨.

## 8-11. 메소드에 예외 선언하기

- 예외를 처리하는 방법
    1. try-catch문으로 직접 처리
    2. 예외 선언하기(예외 떠넘기기, 알리기)
    3. 은폐 - 빈 catch 블럭 만들기
    
- 메소드가 호출시 발생가능한 예외를 호출하는 쪽에 알리는 것 - 예외 선언
    - 참고 - 예외를 발생시키는 키워드 throw와 예외를 메서드에 선언할 때 쓰이는 throws를 잘 구별하자.
    
    ```java
    void method() throws Exception1, Exception2, ... ExceptionN {
    	// 메소드의 내용
    }
    
    // method() 에서 Exception과 그 자손 예외 발생 가능. Exception - 모든 예외의 최고 조상.
    // Exception - 모든 예외가 발생가능.
    void method() throws Exception {
    	// 메소드의 내용
    }
    ```
    
- 메소드에 필수처리예외(체크드예외)만 선언함. 선택 예외는 안 적는 것이 정석이다.

## 8-14 finally 블럭

- 예외 발생여부와 관계없이 수행되어야 하는 코드를 넣는다.
- try 블럭 안에 return문이 있어서 try 블럭을 벗어나갈 때도 finally 블럭이 실행된다.

```java
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch(Exeption1 e1) {
	예외 처리를 위한 문장을 적는다.
} finally {
	// 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
	// finally 블럭은 try-catch 문의 맨 마지막에 위치해야한다.
}
```

## 8-15. 사용자 정의 예외 만들기

- 우리가 직접 예외 클래스를 정의할 수 있다.
- 조상은 Exception(사용자가 발생시키는 예외 - 필수처리 예외라서 트라이캐치를 써야함)과 RuntimeException(프로그래머의 실수로 발생시키는 예외 - 선택처리 예외) 중에서 선택

```java
class MyException extends Exception {
	MyException(String msg) { // 문자열을 매개변수로 받는 생성자
		super(msg); // 조상인 Exception 클래스의 생성자를 호출한다.
	}
}
```

## 8-17. 예외 되던지기(exception re-throwing)

- 예외를 처리한 후에 다시 예외를 발생시키는 것
- 호출한 메소드와 호출된 메소드 양쪽 모두에서 예외처리하는 것.

## 8-18. 연결된 예외(chained exception)

- 한 예외가 다른 예외를 발생시킬 수 있다.
- 예외 A가 예외 B를 발생시키면, A는 B의 원인 예외(cause exception)
    - `Throwable initCause(Throwable cause)` - 지정한 예외를 원인 예외로 등록
    - `Throwable get cause()` - 원인 예외를 반환

```java
void install() throws InstallException {
	try {
		startInstall();
		copyFiles();
	} catch (SpaceException e) {
		InstallException ie = new InstallException("설치 중 예외발생"); // 예외 생성
		ie.initcause(e); // InstallException의 원인 예외를 SpqaceException으로 지정
		throw ie; // InstallException을 발생시킨다.
	} catch (MemoryException me) {
```

- 연결된 예외 사용하는 이유 1 - 여러 예외를 하나로 묶어서 다루기 위해서
- 연결된 예외 사용하는 이유 2 - checked 예외를 unchecked 예외로 변경하려 할 때(필수처리를 선택처리로 바꿀 때 쓴다.)