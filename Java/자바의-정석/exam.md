# 자바의 정석 - 연습 문제 풀이 정리

- [2-1] 다음 표의 빈 칸에 8개의 기본형을 알맞은 자리에 넣으시오.
    - 답
        
        
        |  | 1byte | 2byte | 4byte | 8byte |
        | --- | --- | --- | --- | --- |
        | 논리형 | boolean |  |  |  |
        | 문자형 |  | char |  |  |
        | 정수형 | byte | short | int | long |
        | 실수형 |  |  | float | double |

- [2-2] 주민등록번호를 숫자로 저장하고자 한다. 이 값을 저장하기 위해서는 어떤 자료형
(data type)을 선택해야 할까? regNo라는 이름의 변수를 선언하고 자신의 주민등록번호로
초기화 하는 한 줄의 코드를 적으시오.
    - 답
        
        ```java
        Long regNo = 9703031111111L
        ```
        

- [2-3] 다음의 문장에서 리터럴, 변수, 상수, 키워드를 적으시오.
    
    ```java
    int i = 100;
    long l = 100L;
    final float PI = 3.14f;
    ```
    
    - 답
        
        리터럴 : 100, 100L, 3.14f
        
        변수 : i, l
        
        키워드 : int, long, final, float
        
        상수 : PI
        

- [2-4] 다음 중 기본형(primitive type)이 아닌 것은?
    1. int
    2. Byte
    3. double
    4. boolean
    - 답
        - Byte
        
- [2-5] 다음 문장들의 출력결과를 적으세요. 오류가 있는 문장의 경우. 괄호 안에 ‘오
류’ 라고 적으시오.
    
    ```java
    System.out.println(“1” + “2”) → ( )
    System.out.println(true + “”) → ( )
    System.out.println(‘A' + 'B') → ( )
    System.out.println('1' + 2) → ( )
    System.out.println('1' + '2') → ( )
    System.out.println('J' + “ava”) → ( )
    System.out.println(true + null) → ( )
    ```
    
    - 답
    - 문자열과 덧셈연산을 하면 그 결과는 항상 문자열이 된다.
        
        ```java
        12
        true
        131
        51
        99
        Java
        error
        ```
        
    
- [2-6] 다음 중 키워드가 아닌 것은? (모두 고르시오)
    1. if
    2. Ture
    3. NULL
    4. Class
    5. System
    - 답
        - b, c, d, e

- [2-7] 다음 중 변수의 이름으로 사용할 수 있는 것은? (모두 고르시오)
    
    a. $ystem
    b. channel#5
    c. 7eleven
    d. If
    e. 자바
    f. new
    g. $MAX_NUM
    h. hello@com
    
    - 답
        - a, d, e, g

- [2-8] 참조형 변수와 같은 크기의 기본형은? (모두 고르시오)
    1. int
    2. long
    3. short
    4. float
    5. double
    - 답
        - a, d

- [2-9] 다음 중 형변환을 생략할 수 있는 것은? (모두 고르시오)
    
    ```java
    byte b = 10;
    char ch = 'A';
    int i = 100;
    long l = 1000L;
    ```
    
    a. b = (byte)i;
    b. ch = (char)b;
    c. short s = (short)ch;
    d. float f = (float)l;
    e. i = (int)ch;
    
    - 답
        - d, e
    
- [2-10] char타입의 변수에 저장될 수 있는 정수 값의 범위는? (10 진수로 적으시오)
    - 답
        - 0~65535

- [2-11] 다음 중 변수를 잘못 초기화한 것은? (모두 고르시오)
    
    a. byte b = 256;
    b. char c = '';
    c. char answer = 'no';
    d. float f = 3.14
    e. double d = 1.4e3f;
    
    - 답
        - a, b, c, d

- [2-12] 다음 중 main 메소드의 선언부로 알맞은 것은? (모두 고르시오)
    
    a. public static void main(String[] args)
    b. public static void main(String args[])
    c. public static void main(String[] arv)
    d. public void static main(String[] args)
    e. static public void main(String[] args)
    
    - 답
        - a, b, c, e

- [2-13] 다음 중 타입과 기본값이 잘못 연결된 것은? (모두 고르시오)
    
    a. boolean - false
    b. char - '\u0000'
    c. float - 0.0
    d. int - 0
    e. long - 0
    f. String - ""
    
    - 답
        - c, e, f

- [3-1] 다음 연산의 결과를 적으시오.
    
    ```java
    class Exercise3_1 {
    public static void main(String[] args) {
    int x = 2;
    int y = 5;
    char c = 'A'; // 'A' 65 의 문자코드는
    System.out.println(1 + x << 33);
    System.out.println(y >= 5 || x < 0 && x > 2);
    System.out.println(y += 10 - x++); // y는 15, x는 3
    System.out.println(x+=2);
    System.out.println( !('A' <= c && c <='Z') );
    System.out.println('C'-c);
    System.out.println('5'-'0');
    System.out.println(c+1);
    System.out.println(++c);
    System.out.println(c++);
    System.out.println(c);
    }
    }
    ```
    
    - 답
        
        ```java
        6
        true
        13
        5
        false
        2
        5
        66
        B
        B
        C
        ```
        
    
- [3-2] 아래의 코드는 사과를 담는데 필요한 바구니(버켓)의 수를 구하는 코드이다. 만일 
사과의 수가 123개이고 하나의 바구니에는 10개의 사과를 담을 수 있다면, 13개의 바구니가 필요할 것이다. (1)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise3_2 {
    public static void main(String[] args) {
    int numOfApples = 123; // 사과의 개수
    int sizeOfBucket = 10; // ( ) 바구니의 크기 바구니에 담을 수 있는 사과의 개수
    int numOfBucket = ( /* (1) */ ); // 모든 사과를 담는데 필요한 바구니의 수
    System.out.println("필요한 바구니의 수 :"+numOfBucket); 
    }
    }
    
    // 실행결과
    // 필요한 바구니의 수 :13
    ```
    
    - 답 : 정수간의 나눗셈 연산의 특징은 반올림하지 않고, 버림을 한다는 것. 따라서 나머지 연산자(%)를 사용해서 나머지가 발생하는지 확인해서, 나머지가 발생하면 바구니의 개수에 1을 더해줘야한다.
        
        ```java
        int numOfBucket = numOfApples / sizeOfBucket + (numOfApples % sizeOfBucket > 0 ? 1 : 0)
        ```
        

- [3-3] 아래는 변수 num의 값에 따라 ‘양수', ‘음수', ‘0’을 출력하는 코드이다. 삼항 연산자를 이용해서 (1)에 알맞은 코드를 넣으시오. hint. 삼항 연산자를 두 번 사용.
    
    ```java
    class Exercise3_3 {
    public static void main(String[] args) {
    int num = 10;
    System.out.println( /* (1) */ );
    }
    }
    
    // 실행결과
    // 양수
    ```
    
    - 답 : 삼항 연산자는 두 가지의 경우를 처리할 수 있는데, 삼항 연산자 안에 삼항 연산자를 넣으면 세 가지의 경우를 처리할 수 있게 된다.
        
        ```java
        num > 0 ? "양수" : (num < 0 ? "음수" : "0");
        ```
        

- [3-4] 아래는 변수 num의 값 중에서 백의 자리 이하를 버리는 코드이다. 만일 변수 num의 값이 ‘456’이라면 ‘400’이 되고, ‘111’이라면 ‘100’이 된다. (1)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise3_4 {
    public static void main(String[] args) {
    int num = 456;
    System.out.println( /* (1) */ );
    }
    }
    
    // 실행결과
    // 400
    ```
    
    - 답 : 나눗셈 연산자는 버림을 한다.
        
        ```java
        num/100*100
        ```
        
    
- [3-5] 아래는 변수 num의 값 중에서 일의 자리를 1로 바꾸는 코드이다. 만일 변수 num의 값이 333이라면 331이 되고, 777이라면 771이 된다. (1)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise3_5 {
    public static void main(String[] args) {
    int num = 333;
    System.out.println( /* (1) */ );
    }
    }
    
    // 실행결과
    // 331
    ```
    
    - 답 : num을 10으로 나누면 33이 되고, 10을 곱하면 330이 되고, 거기에 1을 더하면 된다.
        
        ```java
        num / 10 * 10 + 1 
        ```
        

- [3-6] 아래는 변수 num의 값보다 크면서도 가장 가까운 10의 배수에서 변수 num의 값을 뺀 나머지를 구하는 코드이다. 예를 들어, 24의 크면서도 가장 가까운 10의 배수는 30이다. 19의 경우 20이고, 81의 경우 90이 된다. 30에서 24를 뺀 나머지는 6이기 때문에 변수 num의 값이 24라면 6을 결과로 얻어야 한다. (1)에 알맞은 코드를 넣으시오. hint. 나머지 연산자를 사용.
    
    ```java
    class Exercise3_6 {
    public static void main(String[] args) {
    int num = 24;
    System.out.println( /* (1) */ );
    }
    }
    
    // 실행결과
    // 6
    ```
    
    - 답 :
        
        ```java
        (num / 10 + 1) * 10 % num
        
        // 또 다른 답
        10 - num % 10
        ```
        

- [3-7] 아래는 화씨(Fahrenheit) 를 섭씨(Celcius) 로 변환하는 코드이다. 변환공식이 'C =
5/9 ×(F - 32)' 라고 할 때, (1)에 알맞은 코드를 넣으시오. 단, 변환 결과값은 소수점
셋째자리에서 반올림해야한다. (Math.round()를 사용하지 않고 처리할 것)
    
    ```java
    class Exercise3_7 {
    public static void main(String[] args) {
    int fahrenheit = 100;
    float celcius = ( /* (1) */ );
    System.out.println("Fahrenheit:"+fahrenheit);
    System.out.println("Celcius:"+celcius);
    }
    }
    
    // 실행결과
    // Fahrenheit:100
    // Celcius:37.78
    ```
    
    - 답
        
        ```java
        (int)((5 / 9f * (fahrenheit - 32)) * 100 + 0.5) / 100f
        ```
        
    
- [3-8] 아래 코드의 문제점을 수정해서 실행결과와 같은 결과를 얻도록 하시오.
    
    ```java
    class Exercise3_8 {
    public static void main(String[] args) {
    byte a = 10;
    byte b = 20;
    byte c = a + b;
    
    char ch = 'A';
    ch = ch + 2;
    
    float f = 3 / 2;
    long l = 3000 * 3000 * 3000;
    
    float f2 = 0.1f;
    double d = 0.1;
    
    boolean result = d==f2;
    
    System.out.println("c="+c);
    System.out.println("ch="+ch);
    System.out.println("f="+f);
    System.out.println("l="+l);
    System.out.println("result="+result);
    }
    }
    
    // 실행결과
    // c=30
    // ch=C
    // f=1.5
    // l=27000000000
    // result=true
    ```
    
    - 답
        
        ```java
        // a, b는 덧셈연산을 하기 전에 int 타입으로 자동형변환 되기 때문에 byte타입에 담으려면 형변환을 해줘야 한다.
        byte c = (byte)(a + b);
        
        // char 타입도 덧셈연산 과정에서 int 타입으로 자동형변환되기 때문에 char타입 변수에 담으려면 char타입으로 형변환 해줘야 한다.
        ch = (char)(ch + 2);
        
        // 3/2 결과는 1이 된다. 연산결과를 실수로 얻고 싶으면 두 피연산자 중 한 쪽이 실수타입이어야 한다.
        float f = 3 / 2f;
        
        // int*int*int의 결과는 int인데, 최대값이 넘는 결과는 오버플로우가 발생. 그래서 피연산자 중 하나라도 long 타입이어야 함.
        long l = 3000 * 3000 * 3000L;
        
        // float을 double로 형변환할 때 오차가 발생할 수 있다. 따라서 double을 float으로 형변환해서 비교하는 것이 보다 정확함.
        boolean result = (float)d == f2;
        ```
        
    
- [3-9] 다음은 문자형 변수 ch가 영문자(대문자 또는 소문자)이거나 숫자일 때만 변수 b의 값이 true가 되도록 하는 코드이다. (1)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise3_9 {
    public static void main(String[] args) {
    char ch = 'z';
    boolean b = ( /* (1) */ );
    System.out.println(b);
    }
    }
    
    // 실행결과
    // true
    ```
    
    - 답
        
        ```java
        'a' <= ch <= 'z' || 'A' <= ch <= 'Z' || '0' <= ch <= '9'
        ```
        

- [3-10] 다음은 대문자를 소문자로 변경하는 코드인데, 문자 ch에 저장된 문자가 대문자인 경우에만 소문자로 변경한다. 문자코드는 소문자가 대문자보다 32만큼 더 크다. 예를 들어 ‘A’의 코드는 65이고 ‘a’의 코드는 97이다. (1)~(2)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise3_10 {
    public static void main(String[] args) {
    char ch = 'A';
    char lowerCase = ( /* (1) */ ) ? ( /* (2) */ ) : ch;
    System.out.println("ch:"+ch);
    System.out.println("ch to lowerCase:"+lowerCase);
    }
    }
    
    // 실행결과
    // ch:A
    // ch to lowerCase:a
    ```
    
    - 답
        
        ```java
        'A' <= ch && ch <= 'Z' ? (char)(ch + 32) : ch;
        ```
        
    
- [4-1] 다음의 문장들을 조건식으로 표현하라.
    1. int형 변수  x가  10보다 크고 20보다 작을 때 true인 조건식
    2. char형 변수 ch가 공백이나 탭이 아닐 때 true인 조건식
    3. char형 변수 ch가 ‘x’또는 ‘X’일 때 true인 조건식
    4. char형 변수 ch가 숫자(‘0’~‘9’)일 때 true인 조건식
    5. char형 변수 ch가 영문자(대문자 또는 소문자)일 때 true인 조건식
    6. int형 변수 year가 400으로 나눠떨어지거나 또는 4로 나눠떨어지고 100으로 나눠떨어지지
    않을 때 true인 조건식
    7. boolean형 변수 powerOn가 false일 때 true인 조건식
    8. 문자열 참조변수 str이 “yes”일 때 true인 조건식
    - 답
        
        ```java
        // 1
        10 < x && x < 20
        
        // 2
        ch != ' ' && ch != '\t'
        
        // 3
        ch == 'x' || ch == 'X'
        
        // 4
        '0' <= ch && ch <= '9'
        
        // 5
        ('a' <= ch && ch <= 'z') || ('A' <= ch && ch <= 'Z')
        
        // 6
        year % 400 == 0 || (year % 4 == 0 && year % 100 != 0)
        
        // 7
        !powerOn
        
        // 8
        str.equals("yes")
        ```
        
    
- [4-2] 1부터 20까지의 정수 중에서 2 또는 3의 배수가 아닌 수의 총합을 구하시오.
    - 답
        
        ```java
        int result = 0;
        for(int i = 1; i <= 20; i++) {
        	if(i % 2 != 0 && i % 3 != 0) {
        		result += i;
        	}
        }
        System.out.println(result);
        ```
        
    
- [4-3] 1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+...+10)의 결과를 계산하시오.
    - 답
        
        ```java
        int res = 0;
        for(int i = 1; i <= 10; i++) {
        	for(int j = 1; j <= i; j++) {
        		res += j;
        	}
        }
        System.out.println(res);
        ```
        

- [4-4] 1+(-2)+3+(-4)+... 과 같은 식으로 계속 더해나갔을 때, 몇까지 더해야 총합이
100 이상이 되는지 구하시오.
    - 답
        
        ```java
        int sum = 0; // 총합을 저장할 변수
        int s = 1; // 값의 부호를 바꿔주는데 사용할 변수
        int num = 0;
        
        for(int i = 1; true; i++, s = -s) {
        	num = s * i;
        	sum += num;
        			
        	if(sum >= 100) // 총합이 100보다 같거나 크면 반복문을 빠져 나간다.
        		break;
        }
        		
        System.out.println(num);
        System.out.println(sum);
        ```
        

- [4-5] 다음의 for문을 while문으로 변경하시오.
    
    ```java
    public class Exercise4_5 {
    	public static void main(String[] args) {
    		for(int i=0; i<=10; i++) {
    			for(int j=0; j<=i; j++)
    				System.out.print("*");
    			System.out.println();
    		}
    	} // end of main
    } // end of class
    ```
    
    - 답
        
        ```java
        int i = 0;
        		while(i < 10) {
        
        			int j = 0;
        			while(j <= i) {
        				System.out.print("*");
        				j++;
        			}
        			System.out.println();
        			i++;
        		}
        ```
        
- [4-6] 두 개의 주사위를 던졌을 때, 눈의 합이 6이 되는 모든 경우의 수를 출력하는 프로그램을 작성하시오.
    - 답
        
        ```java
        for(int i = 1; i <= 6; i++) {
        	for(int j = 1; j <= 6; j++) {
        		if(i+j == 6) {
        			System.out.println(i + " + " + j + " = 6");
        		}
        	}
        }
        ```
        

- [4-7] Math.random()을 이용해서 1부터 6사이의 임의의 정수를 변수 value에 저장하는 코드를 완성하라. (1)에 알맞은 코드를 넣으시오.
    
    ```java
    class Exercise4_7 {
    public static void main(String[] args) {
    int value = ( /* (1) */ );
    System.out.println("value:"+value);
    }
    }
    ```
    
    - 답
        
        ```java
        int value = (int)(Math.random() * 6) + 1;
        ```
        

- [4-8] 방정식 2x+4y=10의 모든 해를 구하시오. 단, x와 y는 정수이고 각각의 범위는 0≤x≤10, 0≤y≤10이다.
    - 답
        
        ```java
        for(int x = 0; x <= 10; x++) {
        	for(int y = 0; y <= 10; y++) {
        		if(2 * x + 4 * y == 10) {
        			System.out.println("x = " + x + ", y = " + y);
        		}
        	}
        }
        ```
        

- [4-9] 숫자로 이루어진 문자열 str이 있을 때, 각 자리의 합을 더한 결과를 출력하는 코드를 완성하라. 만일 문자열이 “12345”라면, ‘1+2+3+4+5’의 결과인 15를 출력되어야 한다. (1)에 알맞은 코드를 넣으시오. hint. String 클래스의 charAt(int i)를 사용
    
    ```java
    class Exercise4_9 {
    public static void main(String[] args) {
    String str = "12345";
    int sum = 0;
    for(int i=0; i < str.length(); i++) {
    /*
    (1) . 알맞은 코드를 넣어 완성하시오
    */
    }
    System.out.println("sum="+sum);
    }
    }
    ```
    
    - 답
        
        ```java
        sum += str.charAt(i) - '0';
        ```
        

- [4-10] int타입의 변수 num이 있을 때, 각 자리의 합을 더한 결과를 출력하는 코드를 완성하라. 만일 변수 num의 값이 12345라면, ‘1+2+3+4+5’의 결과인 15를 출력하라. (1)에 알맞은 코드를 넣으시오. [주의] 문자열로 변환하지 말고 숫자로만 처리해야 한다.
    
    ```java
    class Exercise4_10 {
    public static void main(String[] args) {
    		int num = 12345;
    		int sum = 0;
    		/*
    		(1) 알맞은 코드를 넣어 완성하시오.
    		*/
    		System.out.println("sum="+sum);
    	}
    }
    ```
    
    - 답
        
        ```java
        int num = 12345;
        int sum = 0;
        
        while(num > 0) {
        	sum += num%10;
        	num /= 10;
        }
        
        System.out.println("sum = " + sum);
        ```

- [4-11] 피보나치(Fibonnaci) 수열(數列)은 앞을 두 수를 더해서 다음 수를 만들어 나가는 수열이다. 예를 들어 앞의 두 수가 1과 1이라면 그 다음 수는 2가 되고 그 다음 수는 1과 2를 더해서 3이 되어서 1,1,2,3,5,8,13,21,... . 과 같은 식으로 진행된다. 1과 1부터 시작하는 피보나치 수열의 10번째 수는 무엇인지 계산하는 프로그램을 완성하시오.
    
    ```java
    public class Exercise4_11 {
    public static void main(String[] args) {
    // Fibonnaci 1, 1 . 수열의 시작의 첫 두 숫자를 로 한다
    int num1 = 1;
    int num2 = 1;
    int num3 = 0; // 세번째 값
    System.out.print(num1+","+num2);
    for (int i = 0 ; i < 8 ; i++ ) {
    	// (1) 알맞은 코드를 넣어 완성하시오.
    }
    } // end of main
    } // end of class
    
    // 실행결과
    // 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
    ```
    
    - 답
        
        ```java
        num3 = num1 + num2;
        System.out.print("," + num3);
        num1 = num2;
        num2 = num3;
        ```
        
    
- [4-12] 구구단의 일부분을 다음과 같이 출력하시오.
    
    ```java
    // 실행결과
    2*1=2 3*1=3 4*1=4
    2*2=4 3*2=6 4*2=8
    2*3=6 3*3=9 4*3=12
    5*1=5 6*1=6 7*1=7
    5*2=10 6*2=12 7*2=14
    5*3=15 6*3=18 7*3=21
    8*1=8 9*1=9
    8*2=16 9*2=18
    8*3=24 9*3=27
    ```
    
    - 답
        
        ```java
        for (int i = 1 ; i <= 9 ; i++) {
        	for (int j = 1; j <= 3; j++) {
        		
        		int x = (j+1) + (i-1) / 3 * 3;
        		int y = i % 3 == 0 ? 3 : i % 3;
        		
        		if(x > 9) {
        			break;
        		}
        		
        		System.out.print(x + "*" + y + "=" + x * y + "\t");
        		
        	}
        
        	System.out.println();
        	
        	if(i % 3 == 0) {
        		System.out.println();
        	}
        	
        }
        ```
        
    

- [4-13] 다음은 주어진 문자열(value)이 숫자인지를 판별하는 프로그램이다. (1)에 알맞은 코드를 넣어서 프로그램을 완성하시오.
    
    ```java
    class Exercise4_13 {
    public static void main(String[] args) {
    	String value = "12o34";
    	char ch = ' ';
    	boolean isNumber = true;
    
    	// charAt(int i) 반복문과 를 이용해서 문자열의 문자를
    	// 하나씩 읽어서 검사한다.
    	for(int i=0; i < value.length() ;i++) {
    		/*
    		(1) 알맞은 코드를 넣어 완성하시오.
    		*/
    	}
    	if (isNumber) {
    		System.out.println(value+" ."); 는 숫자입니다
    	} else {
    		System.out.println(value+" ."); 는 숫자가 아닙니다
    	}
    	} // end of main
    } // end of class
    ```
    
    - 답
        
        ```java
        ch = value.charAt(i);
        			
        if(!(ch >= '0' && ch <= '9')) {
        	isNumber = false;
        	break;
        }
        ```
        

- [4-14] 다음은 숫자맞추기 게임을 작성한 것이다. 1과 100 사이의 값을 반복적으로 입력해서 컴퓨터가 생각한 값을 맞추면 게임이 끝난다. 사용자가 값을 입력하면, 컴퓨터는 자신이 생각한 값과 비교해서 결과를 알려준다. 사용자가 컴퓨터가 생각한 숫자를 맞추면 게임이 끝나고 몇 번 만에 숫자를 맞췄는지 알려준다. (1)~(2)에 알맞은 코드를 넣어 프로그램을 완성하시오.
    
    ```java
    class Exercise4_14
    {
    public static void main(String[] args)
    {
    // 1~100 answer . 사이의 임의의 값을 얻어서 에 저장한다
    int answer = /* (1) */;
    int input = 0; // 사용자입력을 저장할 공간
    int count = 0; // 시도횟수를 세기위한 변수
    // Scanner 화면으로 부터 사용자입력을 받기 위해서 클래스 사용
    java.util.Scanner s = new java.util.Scanner(System.in);
    do {
    count++;
    System.out.print("1 100 :"); 과 사이의 값을 입력하세요
    input = s.nextInt(); // input . 입력받은 값을 변수 에 저장한다
    /*
    (2) . 알맞은 코드를 넣어 완성하시오
    */
    } while(true); // 무한반복문
    } // end of main
    } // end of class HighLow
    ```
    
    - 답
        
        ```java
        // (1)
        int answer = (int)(Math.random() * 100) + 1;
        
        // (2)
        if(answer > input) {
        	System.out.println("더 큰 수를 입력하세요.");
        } else if(answer < input) {
        	System.out.println("더 작은 수를 입력하세요.");
        } else {
        	System.out.println("맞췄습니다.");
        	System.out.println("시도횟수는 " + count + "번입니다.");
        	break;
        }
        ```