# 변수란? 변수의 선언과 저장

## 1. 변수(variable)란?

- 하나의 값을 저장할 수 있는 메모리 공간!

## 2. 변수의 선언
    1. 변수의 선언 이유
        1. 값(data)을 저장할 공간을 마련하기 위해서
    2. 변수의 선언 바업
        1. 변수타입 변수이름;
        2. int age; → 정수(int)타입의 변수 age를 선언

## 3. 변수에 값 저장하기
    1. 변수에 값 저장하기(’=’는 등호가 아니라 대입)
        1. `int age;` - 정수(int)타입의 변수 age를 선언
        2. `age = 25;` - 변수 age에 25를 저장
        3. `int age = 25;` - 위의 두 줄을 한 줄로
    2. 변수의 초기화 - 변수에 처음으로 값을 저장하는 것
        1. `int x = 0;` - 변수 x를 선언 후, 0으로 초기화
        2. `int y = 5;` - 변수 y를 선언 후, 5로 초기화
        3. `int x = 0, y = 5;` - 위의 두 줄을 한 줄로
        4. 지역 변수는 읽기 전에 꼭! 초기화해야 함!

## 4. 변수의 값 읽어오기
    1. 변수의 값이 필요한 곳에 변수의 이름을 적는다.
        1. `int year = 0, age 14;`
        2. `year = age + 2000;` → `year = 14 + 2000;`
        3. `year = 2014;`
        4. `age = age + 1;` → `age = 14 + 1;` → `age = 15;`

# 변수의 타입

## 변수의 타입

1. 변수의 타입은 저장할 값의 타입에 의해 결정된다.
    - `int age = 24;`
    - `age = 3.14;` 는 불가능.
2. 저장할 값의 타입과 일치하는  타입으로 변수를 선언
    - `char ch = ‘가’` : char는 문자 타입
    - `double pi = 3.14;` : double은 실수 타입

## 값의 타입

1. 값
    1. 문자 - 가나다, ABC : char 타입
    2. 숫자
        1. 정수 - 0, 25, -100 : byte, short, int, long 타입
        2. 실수 - 3.14, -0.1 : float, double 타입
    3. 논리 - boolean : true, false 타입
- 기본형  : char, byte, short, int, long, float, double, boolean

# 상수와 리터럴

## 변수, 상수, 리터럴

- 변수(variable) - 하나의 값을 저장하기 위한 공간
- 상수(constant) - 한 번만 값을 저장 가능한 변수
    - `final int MAX = 100;`
- 리터럴(literal) - 그 자체로 값을 의미하는 것

```java
public class VarEx3 {
	public static void main(String[] args) {

		final int scroe = 100;
		score = 200; // 컴파일 에러 발생. score는 상수이기 때문
	}
}
```

# 리터럴의 타입과 접미사
## 리터럴의 접두사와 접미사

| 종류 | 리터럴 | 접미사 |
| --- | --- | --- |
| 논리형 | false, true | 없음 |
| 정수형 | 123, 0b0101, 077, 0xFF, 100L | L |
| 실수형 | 3.14, 3.0e8, 1.4f, 0x1.0p-1 | f, d |
| 문자형 | ‘A’, ‘1’, ‘\n’ | 없음 |
| 문자열 | “ABC”, “123”, “A”, “true” | 없음 |

## 변수와 리터럴의 타입 불일치

1. 범위가 변수 > 리터럴 인 경우, OK
    1. `int i = ‘A’; // int > char`
    2. `long l = 123; // long > int`
    3. `double d = 3.14f; // double > float`
2. 범위가 변수 < 리터럴 인 경우, 에러
    1. `int i = 3000000000; // int의 범위 (+-20억) 벗어남`
    2. `long l = 3.14f; // long < float`
    3. `float f = 3.14; // float < double`
3. byte, short 변수에 int 리터럴 저장 가능
    1. 단, 변수의 타입의 범위 이내여야 함
        1. byte b = 100; // OK. byte의 범위(-128 ~ 127)에 속함
        2. byte b = 128; // 에러. byte의 범위를 벗어남

# 문자, 문자열 리터럴, 문자열 결합
## 문자와 문자열

`char ch = ‘A’;`

`char ch = ‘AB’; // 에러`

`String s = “ABC”; // 문자열`

- String은 클래스지만 문자열은 자주 쓰이기 때문에 기본형 변수에 값을 저장하는 표현을 허용.
    - String s1 = “AB”;
    - String s2 = new String(”AB”);
        - 위 두 문장은 거의 비슷하지만 다르다.

- 문자열 = 연속된 여러 문자지만, 문자열이 하나거나 없어도 괜찮다.
    - `String s = “A”;`
    - `String s = “”;` // 빈 문자열
    - `char ch = ‘’;` // 에러
    - `String s1 = “A” + “B”; // “AB”`
    - `“” + 7` → `“” + “7”` → `“7”`
        - 숫자 → 문자열을 더하면 숫자는 자동으로 문자열로 변환됨.
        - 문자열 결합은 왼쪽에서 오른쪽으로 진행

## 두 변수의 값 교환하기

```java
int x = 10, y = 20;
int tmp; // 빈 컵
tmp = x; // x의 값을 tmp에 저장
x = y; // y의 값을 x에 저장
y = tmp; // tmp(x)의 값을 y에 저장
```

# 기본형과 참조형
## 값의 타입

- 값(data) 의 타입 기본형 8개
    - 문자(가나다, ABC) - char
    - 숫자
        - 정수(0, 25, -100) - byte, short, int, long
        - 실수(3.14, -0.1) - float, double
    - 논리 - boolean(true, false)

변수의 타입은 기본형과 참조형으로 나뉜다.

- 기본형(Primitive type)
    - 논리형, 문자형, 정수형, 실수형
    - 실제 값을 저장
- 참조형(Reference type)
    - 기본형을 제외한 나머지(String, System 등) - 무한개
    - 메모리 주소를 저장함(4byte 또는 8byte)
        - 메모리 주소는 0부터 1씩 증가하는 부호 없는 정수(즉, 양수)
    - `Date today;` // 참조형 변수 today를 선언
    - `today = new date();` // today에 객체의 주소를 저장

# 기본형의 종류와 범위
## 기본형(Primitive type) - 종류와 크기

- 논리형(boolean) - true와 false 중 하나를 값으로 가지며, 조건식과 논리적 계산에 사용된다.
- 문자형(char) - 문자를 저장하는데 사용되며, 변수 당 하나의 문자만을 저장할 수 있다.
- 정수형(int, long) - 정수 값을 저장하는데 사용된다. 주로 사용하는 것은 int와 long이며, byte는 이진 데이터를 다루는데 사용되며, short은 c언어와의 호환을 위해 추가되었다.(잘 안쓰임)
- 실수형 - 실수 값을 저장하는데 사용된다. float과 double이 있다.

1bit = 2진수 1자리

1byte = 8bit

| 종류 / 크기(byte) | 1 | 2 | 4 | 8 |
| --- | --- | --- | --- | --- |
| 논리형 | boolean |  |  |  |
| 문자형 |  | char |  |  |
| 정수형 | byte | short | int | long |
| 실수형 |  |  | float | double |

## 기본형(Primitive type) - 표현 범위(1/3)

- `byte b;`
- `b = 3;` → 1byte : 8bit / 1bit : 2진수 한 자리라서 0또는 1 → 00000011

1bit - 2진수 한 자리에 저장할 수 있는 값은 0과 1. 두 개 뿐.

2bit - 저장할 수 있는 값은 네 개.

n비트로 표현할 수 있는 값의 개수 : 2의 n제곱 개

8비트는 256개.

n비트로 표현할 수 있는 부호없는 정수의 범위 : 0 ~ 2의 n제곱 - 1

따라서, 8비트는 0~255개.

n비트로 표현할 수 있는 부호있는 정수의 범위 : -2의 n-1제곱 ~ 2의 n-1제곱 -1

## 기본형(Primitive type) - 표현 범위(2/3)

## 기본형(Primitive type) - 표현 범위(3/3)

| 자료형 | 저장 가능한 값의 범위(양수) | 정밀도 | 크기 - bit | 크기- byte |
| --- | --- | --- | --- | --- |
| float | 1.4E-45 ~ 3.4E38 | 7자리 | 32 | 4 |
| double | 4.9E-324 ~ 1.8E308 | 15자리 | 64 | 8 |

# printf를 이용한 출력
## 형식화된 출력 - printf()

- println() 의 단점 - 출력형식 지정불가
    1. 실수의 자리수 조절 불가 - 소수점 n자리만 출력하려면?
        1. `System.out.println(10.0/3); // 3.33333333...`
    2. 10진수로만 출력된다. - 8진수, 16진수로 출력하려면?
        1. `System.out.println(0x1A); // 26`
    
- printf()로 출력형식 지정 가능
    - `System.out.printf(”%.2f”, 10.0/3); // 3.33`
    - `System.out.printf(”%d”, 0x1A); // 26`
    - `System.out.printf(”%X”, 0x1A); // 1A`

## printf()의 지시자(1/3)

| 지시자 | 설명 |
| --- | --- |
| %b | 불리언(boolean) 형식으로 출력 |
| %d | 10진(decimal) 정수의 형식으로 출력 |
| %o | 8진(octal) 정수의 형식으로 출력 |
| %x, %X | 16진(hexa-decimal) 정수의 형식으로 출력 |
| %f | 부동 소수점(floating-point)의 형식으로 출력 |
| %e, %E | 지수(exponent) 표현식의 형식으로 출력 |
| %c | 문자(character)로 출력 |
| %s | 문자열(string)로 출력 |
- `System.out.printf(”age:%d year:%d\n”, 14, 2017);`
    - “age:14 year:2017\n”이 화면에 출력된다.

## printf()의 지시자(2/3)

1. 정수를 10진수, 8진수, 16진수로 출력
    1. `System.out.printf(”%d”, 15); // 15 10진수`
    2. `System.out.printf(”%o”, 15); // 17 8진수`
    3. `System.out.printf(”%x”, 15); // f 16진수`
    4. `System.out.printf(”%s”, Integer.toBinaryString(15)); // 1111 2진수`
2. 8진수와 16진수에 접두사 붙이기 - 8진수의 접두사는 0, 16진수의 접두사는 0x
    1. #을 사이에 집어넣으면 됨.
    2. `System.out.printf(”%#o”, 15); // 017`
    3. `System.out.printf(”%#x”, 15); // 0xf`
    4. `System.out.printf(”%#X”, 15); // 0XF`
3. 실수 출력을 위한 지시자 %f - 지수형식(%e), 간략한 형식(%g)
    1. `float f = 123.4567890f;`
    2. `System.out.printf(”%f”, f); // 123.456787`
    3. `System.out.printf(”%e”, f); // 1.234568e+02`
    4. `System.out.printf(”%g”, 123.456789); // 123.457`
    5. `System.out.printf(”%g”, 0.00000001); // 1.00000e-8`

## printf()의 지시자(3/3)

- `System.out.printf(”[%5d]%n”, 10); // [   10]`
- `System.out.printf(”[%-5d]%n”, 10); // [10   ]`
- `System.out.printf(”[%05d]%n”, 10); // [00010]`
- `System.out.printf(”d=%14.10f%n”, d); // 전체 14자리 중 소수점 아래 10자리`

- `System.out.printf(”[%s]%n”, url); // 문자열 출력`
- `System.out.printf(”[%20s]%n”, url); // 20자리 왼쪽 공백 문자열 출력`
- `System.out.printf(”[%-20s]%n”, url); // 20자리 오른쪽 공백 문자열 출력`
- `System.out.printf(”[%.8s]%n”, url); // 부분 출력`

## 화면에서 입력받기 - Scanner

- Scanner란?
    - 화면으로부터 데이터를 입력받는 기능을 제공하느 클래스
- Scanner를 사용하려면
    - import문 추가
        - `import java.util.*;`
    - Scanner 객체의 생성
        - `Scanner scanner = new Scanner(System.in);`
    - Scanner 객체를 사용
        - `int num = scanner.nextInt();` // 화면에서 입력받은 정수를 num에 저장
        - `String input = scanner.nextLine();` // 화면에서 입력받은 내용을 input에 저장
        - `int num = Integer.parseInt(input)` // 문자열(input)을 숫자(num)로 변환

## 오버플로우 : 표현 가능한 범위를 넘는 것

- 최대값 + 1 → 최소값
- 최소값 - 1 → 최대값

## 타입 간의 변환 방법

1. 문자와 숫자 간의 변환
    1. 숫자 → 문자 변환은 문자 0을 더한다.
    2. 문자 → 숫자 변환은 문자 0을 뺀다.
2. 문자열로의 변환
    1. 숫자 → 문자열 변환은 빈 문자열을 더한다.
    2. 문자 → 문자열 변환은 빈 문자열을 더한다.
3. 문자열을 숫자로 변환
    1. 문자열 → 숫자 변환은 `Integer.parseInt("3");` 
    2. 실수 문자열 → 숫자 변환은 `Double.parseDouble("3.4");`
4. 문자열을 문자로 변환
    1. 문자열 → 문자 변환은 `“3”.charAt(0)`