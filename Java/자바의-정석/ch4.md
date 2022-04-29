## 조건문과 반복문

- 조건문 : 조건을 만족할 때만 {}를 수행(0~1번)
    
    ```java
    if(score > 60) {
    	System.out.println("합격입니다.");
    }
    ```
    
- 반복문 : 조건을 만족하는 동안 {}를 수행(0~n번)
    
    ```java
    int i = 10;
    while(i-- > 0) {
    	System.out.pritnln(i);
    }
    ```
    

- 조건문과 반복문은 제어문이라고 한다. (flow control statement)

## if문

- 조건식이 참(true)일 때, 괄호{} 안의 문장들을 수행한다.
    
    ```java
    if(조건식) {
    	// 조건식이 참(true)일 때 수행될 문장들을 적는다.
    }
    ```
    

## 블럭 {}

- 여러 문장을 하나로 묶어주는 것
    
    ```java
    if(score > 60) { // 블럭의 시작
    	// 탭에 의한 들여쓰기
    	System.out.println("합격입니다.");
    } // 블럭의 끝
    ```
    
- 블럭을 생략하면 아래 문장 하나만 속함. 되도록이면 괄호 생략하지 않는 것이 좋다.

## if-else 문

- 둘 중의 하나 - 조건식이 참일 때와 거짓일 때로 나눠서 처리
    
    ```java
    if(조건식) {
    	// 조건식이 참(true)일 때 수행될 문장들을 적는다.
    } else {
    	// 조건식이 거짓(false)일 때 수행될 문장들을 적는다.
    }
    ```

## if-else if문

- 여러 개 중의 하나 - 여러 개의 조건식을 포함한 조건식
    
    ```java
    if(조건식1) {
    	// 조건식1의 연산결과가 참일 때 수행될 문장들을 적는다.
    } else if(조건식2) {
    	// 조건식2의 연산결과가 참일 때 수행될 문장들을 적는다.
    } else if(조건식3) { // 여러 개의 else if를 사용할 수 있다.
    	// 조건식3의 연산결과가 참일 때 수행될 문장들을 적는다.
    } else { // 마지막은 보통 else 블럭으로 끝나며, else 블럭은 생략 가능하다.
    	// 위의 어느 조건식도 만족하지 않을 때 수행될 문장들을 적는다.
    }
    ```
    

## 중첩 if문 - if문 안의 if

```java
if(조건식1) {
	// 조건식1의 연산결과가 true일 때 수행될 문장들을 적는다.
	if(조건식2) {
		// 조건식1과 조건식2가 모두 true일 때 수행될 문장들
	} else {
		// 조건식1이 true이고, 조건식2가 false일 때 수행되는 문장들
	}
} else {
	// 조건식1이 false일 때 수행되는 문장들
}
```

## switch문

- 처리해야 하는 경우의 수가 많을 때 유용한 조건문
    
    ```java
    switch(조건식) {
    	case 값1 :
    					// 조건식의 결과가 값1과 같을 경우 수행될 문장들
    					break;
    	case 값2 :
    					// 조건식의 결과가 값2와 같을 경우 수행될 문장들
    					break; // switch문을 벗어난다.
    	default : // default 생략 가능
    					// 조건식의 결과와 일치하는 case문이 없을 때 수행될 문장들
    }
    ```
    
    1. 조건식을 계산한다.
    2. 조건식의 결과와 일치하는 case문으로 이동한다.
    3. 이후의 문장들을 수행한다.
    4. break문이나 switch문의 끝을 만나면 switch문 전체를 빠져나간다.
- switch의 조건식 결과는 정수, 문자열이 들어갈 수 있다. if의 조건식 결과는 true, false만 가능. 경우의 수가 너무 많아 복잡하면 switch로 사용할 수 없는지 고민하기. switch문은 항상 if문으로 바꿀 수 있는데, if문은 제약조건 때문에 swtich문으로 항상 바꿀 순 없다.

## swtich문의 제약 조건

1. switch문의 조건식 결과는 정수 또는 문자열(jdk 1.7부터 가능)이어야 한다.
2. case문의 값은 정수 상수(문자 포함), 문자열만 가능하며, 중복되지 않아야 한다.

## 임의의 정수(난수) 만들기(실수도 가능)

- `Math.random()`
    - 0.0과 1.0사이의 임의의 double값을 반환
        - 0.0 ≤ Math.random() < 1.0
        - 0.0 ~ 0.9999999... 반환
    - 1~10 사이의 난수 구하기
        
        ```java
        System.out.println((int)(Math.random() * 10) + 1);
        ```
        
    - -5~5 사이의 난수 구하기
        
        ```java
        System.out.println((int)(Math.random() * 11) - 5);
        ```

## for문

- 조건을 만족하는 동안 블럭{}을 반복 - 반복 횟수를 알 때 적합
    
    ```java
    for(초기화; 조건식; 증감식) {
    	수행될 문장
    }
    ```
    
    1. 초기화
    2. 조건식
    3. 수행될 문장 수행
    4. 증감식 → 조건식으로 이동

```java
for(int i = 1; i <= 5; i++) {
	System.out.println("I can do it.");
}
```

- 반복문에서 조건식은 주의해서 작성해야함 - 무한 반복할 수 있음.
- scope(범위) - 선언 위치부터 선언된 블럭의 끝까지
- 조건식을 생략하면, true로 간주되어서 무한반복문이 됨.
  
## 중첩 for문

- for문 내에 또 다른 for문을 포함시킬 수 있다.
    
    ```java
    // 구구단 2단~9단 출력
    for(int i = 2; i <= 9; i++) {
    	for(int j = 1; j <= 9; j++) {
    		System.out.println(i+"*"+j+"="+(i*j));
    	}
    }
    ```
    

- 별 찍기
    
    ```java
    for(int i = 1; i <= 10; i++) {
    	for(int j = 1; j <= 10; j++) {
    		System.out.print("*");
    	}
    	System.out.println();
    }
    ```

## while문

- 조건을 만족시키는 동안 블럭{}을 반복 - 반복횟수 모를 때
    - for문은 반복횟수를 알 때 사용.

```java
while(조건식) {
	// 조건식의 연산결과가 참(true)인 동안, 반복될 문장들을 적는다.
}
```

조건식이 false일 때까지 블럭을 반복한다. 조건식이 false이면 문장이 수행되지 않는다.

## do-while문

- 블럭{}을 최소한 한 번 이상 반복 - 사용자 입력받을 때 유용

```java
do {
	// 조건식의 연산결과가 참일 때 수행될 문장들을 적는다.(처음 한 번은 무조건 실행)
} while(조건식); // 끝에 ';'을 잊지 않도록 주의
```

## break문

- 자신이 포함된 하나의 반복문을 벗어난다.

```java
int sum = 0;
int i = 0;

while(true) { // 무한 반복문 for(;;) {}
	if(sum > 100)
		break;
	++i;
	sum += i;
} // end of while

System.out.println("i=" + i);
System.out.println("sum=" + sum);
```

## continue문

- 자신이 포함된 반복문의 끝으로 이동 - 다음 반복으로 넘어감. 전체 반복 중에서 특정 조건 시 반복을 건너 뛸 때 유용

```java
for(int i = 0; i <= 10; i++) {
	if(i%3==0)
		continue;
	System.out.println(i);
}
// 결과 1, 2, 4, 5, 7, 8, 10
```

## 이름 붙은 반복문

- 반복문에 이름을 붙여서 하나 이상의 반복문을 벗어날 수 있다.

```java
Loop1 : for(int i = 2; i <= 9; i++) {
	for(int j = 1; j <= 9; j++) {
		if(j==5)
			break Loop1:
		System.out.println(i+"*"+j+"="+i*j);
	} // end of for i
	System.out.println();
} // end of Loop1
```

