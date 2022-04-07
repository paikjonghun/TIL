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
    

## 9-11. String 클래스의 생성자와 메소드

```java
String(String s) // 주어진 문자열(s)을 갖는 String 인스턴스를 생성한다.

String(char[] value) // 주어진 문자열(value)을 갖는 String 인스턴스를 생성한다.

String(StringBuffer buf) // StringBuffer 인스턴스가 갖고 있는 문자열과 같은 내용의 String 인스턴스를 생성한다.

char charAt(int index) // 지정된 위치(index)에 있는 문자를 알려준다.(index는 0부터 시작)

int compareTo(String str) // 문자열(str)과 사전순서로 비교한다. 같으면 0을, 사전순으로 이전이면 음수를, 이후면 양수를 반환한다. 문자열을 정렬할 때 사용.

String concat(String str) // 문자열(str)을 뒤에 덧붙인다.

boolean contains(CharSequence s) // 지정된 문자열(s)이 포함되었는지 검사한다.
// CharSequence 는 인터페이스. 인터페이스의 장점 - 서로 관계없는 클래스들의 관계를 맺어줌.

boolean endsWith(String suffix) // 지정된 문자열(suffix)로 끝나는지 검사한다.

boolean equals(Object obj) // 매개변수로 받은 문자열(obj)과 String 인스턴스의 문자열을 비교한다. obj가 String이 아니거나 문자열이 다르면 false를 반환한다.

boolean equalsIgnoreCase(String str) // 문자열과 String 인스턴스의 문자열을 대소문자 구분없이 비교한다.

int indexOf(int ch) // 주어진 문자(ch)가 문자열에 존재하는지 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환한다.(index는 0부터 시작)

int indexOf(int ch, int pos) // 주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)부터 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환한다.(index는 0부터 시작)

int indexOf(String str) // 주어진 문자열이 존재하는지 확인하여 그 위치(index)를 알려준다. 없으면 -1을 반환한다.(index는 0부터 시작)

int lastIndexOf(int ch) // 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 알려준다. 못찾으면 -1을 반환한다.

int lastIndexOf(String str) // 지정된 문자열을 인스턴스의 문자열 긑에서부터 찾아서 위치(index)를 알려준다. 못 찾으면 -1을 반환한다.

int length() // 문자열의 길이를 알려준다.

String[] split(String regex) // 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다.

String[] split(String regex, int limit) // 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다. 단, 문자열 전체를 지정된 수(limit)로 자른다.

boolean startsWith(String prefix) // 주어진 문자열(prefix)로 시작하는지 검사한다.

String substring(int begin)
String substring(int begin, int end)
// 주어진 시작위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻는다. 이 때, 시작위치의 문자는 범위에 포함되지만, 끝 위치의 문자는 포함되지 않는다.

String toLowerCase() // String 인스턴스에 저장되어있는 모든 문자열을 소문자로 변환하여 반환한다.

String toUpperCase() // String 인스턴스에 저장되어있는 모든 문자열을 대문자로 변환하여 반환한다.

String trim() // 문자열의 왼쪽 끝과 오른 쪽 끝에 있는 공백을 없앤 결과를 반환한다. 이 때 문자열 중간에 있는 공백은 제거 되지 않는다.

static String valueOf(boolean b)
static String valueOf(char c)
static String valueOf(int i)
static String valueOf(long l)
static String valueOf(float f)
static String valueOf(double d)
static String valueOf(Object o)
// 지정된 값을 문자열로 변환하여 반환한다. 참조변수의 경우, toString()을 호출한 결과를 반환한다.
```

## 9-12~14. Join()과 StringJoiner

- join()은 여러 문자열 사이에 구분자를 넣어서 결합한다.

```java
String animals = "dog,cat,bear";
String[] arr = animals.split(","); // 문자열을 ','를 구분자로 나눠서 배열에 저장
String str = String.join("-", arr); // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str); // dog-cat-bear
```

## 9-13. 문자열과 기본형 간의 변환

- 숫자를 문자열로 바꾸는 방법

```java
int i = 100;
String str1 = i + ""; // 100을 "100"으로 변환하는 방법1
String str2 = String.valueOf(i); // 100을 "100"으로 변환하는 방법2
```

- 문자열을 숫자로 바꾸는 방법

```java
int i = Integer.parseInt("100"); // "100"을 100으로 변환하는 방법1
int i2 = Integer.valueof("100"); // "100"을 100으로 변환하는 방법2
Integer i2 = Integer.valueOf("100"); // 원래는 반환 타입이 Integer
```

## 9-15. StringBuffer 클래스 - 문자열을 저장 & 다루기위한 클래스

- String처럼 문자형 배열(char[])을 내부적으로 가지고 있다.
    
    ```java
    public final class StringBuffer implements java.io.Serializable {
    	private char[] value;
    	...
    }
    ```
    
- 그러나, String과 달리 내용을 변경할 수 있다.(mutable)
- 문자열 추가, 변경 많이 하면 StringBuffer를 사용하는 것이 좋다.

## 9-16. StringBuffer의 생성자

- 배열은 길이 변경 불가. 공간이 부족하면 새로운 배열 생성해야함
- StringBuffer는 저장할 문자열의 길이를 고려해서 적절한 크기로 생성해야함

## 9-17. StringBuffer의 변경

- StringBuffer는 String과 달리 내용 변경이 가능하다.
    
    ```java
    StringBuffer sb = new StringBuffer("abc");
    sb.append("123"); // sb의 내용 뒤에 "123"을 추가한다.
    ```
    
- append()는 지정된 내용을 StringBuffer에 추가 후, StringBuffer의 참조를 반환

## 9-18. StringBuffer의 비교

- StringBuffer는 equals()가 오버라이딩 되어있지 않다. (주소 비교)
- StringBuffer를 String으로 변환 후에 equals()로 비교해야 한다.

## 9-19. StringBuffer의 생성자와 메소드

```java
StringBuffer() // 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다.

StringBuffer(int length) // 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다.

StringBuffer(String str) // 지정된 문자열 값(str)을 갖는 StringBuffer 인스턴스를 생성한다.

StringBuffer append(boolean b)
StringBuffer append(char c)
StringBuffer append(char[] str)
StringBuffer append(double d)
StringBuffer append(float f)
StringBuffer append(int i)
StringBuffer append(long l)
StringBuffer append(Object obj)
StringBuffer append(String str) // 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다.

int capacity() // StringBuffer 인스턴스의 버퍼 크기를 알려준다. length()는 버퍼에 담긴 문자열의 길이를 알려준다.

char charAt(int index) // 지정된 위치(index)에 있는 문자를 반환한다.

StringBuffer delete(int start, int end) // 시작위치(start)부터 끝 위치(end) 사이에 있는 문자를 제거한다. 단, 끝 위치의 문자는 제외.

StringBuffer deleteCharAt(int index) // 지정된 위치(index)의 문자를 제거한다.

StringBuffer insert(int pos, boolean b)
StringBuffer insert(int pos, char c)
StringBuffer insert(int pos, char[] str)
StringBuffer insert(int pos, double d)
StringBuffer insert(int pos, float f)
StringBuffer insert(int pos, int i)
StringBuffer insert(int pos, long l)
StringBuffer insert(int pos, Object obj)
StringBuffer insert(int pos, String str) // 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가한다. pos는 0부터 시작 .

StringBuffer insert(int pos, String str) // 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가한다. pos는 0부터 시작

int length() // StringBuffer 인스턴스에 저장되어 있는 문자열의 길이를 반환한다.

StringBuffer relplace(int start, int end, String str) // 지정된 범위(start ~ end)의 문자들을 주어진 문자열로 바꾼다. end위치의 문자는 범위에 포함되지 않음

StringBuffer reverse() // StringBuffer인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다.

void setCharAt(int index, char ch) // 지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.

void setLength(int newLength) // 지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈 공간을 널문자'\u0000' 로 채운다.

String toString() // StringBuffer 인스턴스의 문자열을 String으로 반환

String substring(int start)
String substring(int start, int end) // 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다. 시작위치만 지정하면 시작위치부터 문자열 끝까지 뽑아서 반환한다.
```