## 9-1. Object 클래스

- 모든 클래스의 최고 조상. 오직 11개의 메소드만을 가지고 있다.
- notify(), wait() 등은 쓰레드와 관련된 메소드이다.

## 9-2. equals(Object obj)

- 객체 자신(this)과 주어진 객체(obj)를 비교한다. 같으면 true, 다르면 false.
- Object 클래스의 equals()는 객체의 주소를 비교(참조변수 값 비교)

## 9-3. equals(Object obj)의 오버라이딩

- 인스턴스 변수(iv)의 값을 비교하도록 equals()를 오버라이딩해야 한다.

## 9-4. hashCode()

- 객체의 해시코드(hash code)를 반환하는 메소드 - 해시코드는 정수값. 해싱이라는 알고리즘에서 사용함.
- Object 클래스의 hashCode()는 객체의 주소를 int로 변환해서 반환.
    - 네이티브 메소드 : OS의 메소드(C언어)
    
    ```java
    public class Object {
    	...
    	public native int hashCode();
    ```
    
    - OS가 가지고 있는 메소드를 사용하기 때문에 내용이 없음
- 객체의 지문이라고도 함. 객체 마다 주소값이 다르기 때문.
- equals()를 오버라이딩하면, hashCode()도 오버라이딩해야 함.
    - equals()의 결과가 ture인 두 객체의 해시코드는 같아야 하기 때문!
- System.identityHashCode(Object obj)는 Object클래스의 hashCode()와 동일
    - 객체마다 다른 해시코드 반환하는 메소드. hashCode()를 오버라이딩하고 같은 기능이 필요할 때 사용.
    

## 9-5~6. toString(), toString()의 오버라이딩

- toString() : 객체를 문자열(String)으로 변환하기 위한 메소드

```java
public String toString() { // Object클래스의 toString()
	// 설계도객체. 클래스이름. @는 위치. 16진수로 문자열을 바꾸는 것.
	return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

- 객체==iv집합 이므로 객체를 문자열로 변환한다는 것은 iv의 값을 문자열로 변환한다는 것과 같다.

## 9-7. String 클래스

- String 클래스 - 문자열을 다루기 위한 클래스.
    - 데이터(char[]) - 문자 배열 + 메소드(문자열 관련)을 가지고 있다.
    
    ```java
    public final class String implements java.io.Serializable, Comparable {
    	private char[] value;
    ```
    
- 내용을 변경할 수 없는 불변(immutable) 클래스
- 덧셈 연산자(+)를 이용한 문자열 결합은 성능이 떨어짐. 문자열의 결합이나 변경이 잦다면, 내용을 변경가능한 StringBuffer를 사용.

## 9-8. 문자열의 비교

- `String str = “abc”`와 `String str = new String(”abc”);`의 비교

## 9-9. 문자열 리터럴

- 문자열 리터럴은 프로그램 실행시 자동으로 생성된다.(constant pool상수 저장소에 저장)
- 같은 내용의 문자열 리터럴은 하나만 만들어진다.(문자열 리터럴도 String 객체다)

## 9-10. 빈 문자열(””, empty string)

- 내용이 없는 문자열. 크기가 0인 char형 배열을 저장하는 문자열
    
    ```java
    String str = ""; // str을 빈 문자열로 초기화
    ```
    
- 크기가 0인 배열을 생성하는 것은 어느 타입이나 가능
    
    ```java
    char[] chArr = new char[0]; // 길이가 0인 char배열
    int[] iArr = {}; // 길이가 0인 int배열
    ```
    
- 문자(char)와 문자열(String)의 초기화
    
    ```java
    String s = ""; // 빈 문자열로 초기화
    char c = ' '; // 공백으로 초기화
    ```