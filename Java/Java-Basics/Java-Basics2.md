## 목차
1. 더하기의 법칙
2. Switch
3. 실습1
4. 다항 연산
5. 반복문
6. 실습2

---

## 1. 더하기의 법칙
- `숫자 + 숫자 = 숫자`
- `문자열 + 문자열 = 문자열` -> +가 이어준다는 역할 ( “Hello” + “World” -> “HelloWorld”
- `문자열 + 숫자 = 문자열`
- `숫자 + 문자열 = 문자열`
  - 숫자10 + 숫자20 + 문자열”30” = “3030”
> 1. 숫자와 문자열간 더하기는 문자열을 우선으로 함
> 2. 연산은 앞에서부터 차례대로 진행

<br/>

## 2. Switch

```java
switch(값) {
case 값 1 : 내용1
case 값 2 : 내용2
…
default : 내용n
}
```

1. 값과 값1이 같으면 내용1 이후를 실행한다
2. 값과 값2가 같으면 내용2 이후를 실행한다
3. 모든 case 값과 같지 않으면 defalut를 실행한다.
4. if문은 범위 형태의 조건이 가능데, switch는 정확하게 값이 정해진 조건만 가능하다.
5. break; 해당 구문을 종료한다. break; 를 switch 안에 넣으면 switch를 종료.


> if문 : 조건을 작성할 때에는 작은 기준에서 큰 기준으로 해야 모든 기준을 돈다.

<br/>

## 3. 실습 1

- 조건문을 활용하여 학점을 출력하시오.

```java
int score = 40;

if(score >= 90) {

System.out.println("if - 학점은 A입니다.");

} else if(score >= 80) {

System.out.println("if - 학점은 B입니다.");

} else if(score >= 70) {

System.out.println("if - 학점은 C입니다.");

} else if(score >= 60) {

System.out.println("if - 학점은 D입니다.");

} else {

System.out.println("if - 학점은 F입니다.");

}
```

- switch 활용

```java
int score2 = score / 10;

switch(score2) {

case 10 :

case 9 : System.out.println("switch - 학점은 A입니다.");

break;

case 8 : System.out.println("switch - 학점은 B입니다.");

break;

case 7 : System.out.println("switch - 학점은 C입니다.");

break;

case 6 : System.out.println("switch - 학점은 D입니다.");

break;

default : System.out.println("switch - 학점은 F입니다.");

// A+ = 95 ~ 100
// A = 90 ~ 94
// B+ = 85 ~ 89
// B = 80 ~ 84
// C+ = 75 ~ 79
// C = 70 ~ 74
// D+ = 65 ~ 69
// D = 60 ~ 64
// F = 나머지

switch(score2) {

case 20 :

case 19 : System.out.println(“A+”);

break;

case 18 : System.out.println(“A”);

braek;

case 17 : System.out.println(“B+”);

break;

case 16 : System.out.println(“B”);

break;

case 15 : System.out.println(“C+”);

break;

case 14 : System.out.println(“C”);

break;

case 13 : System.out.println(“D+”);

break;

case 12 : System.out.println(“D”);

break;

default : System.out.println(“F”);
```

<br/>

## 4. 다항 연산
- `&` -> ‘엔퍼센트’ 라고 읽음
- `|` -> ‘파이프라인’ 이라고 읽음.
- `A && B` -> AND -> A와 B가 모두 true 일 때만 true 다. **교집합**
- `A || B` -> OR -> A와 B가 하나라도 true 이면 true 다. **합집합**
> AND인지 OR인지 헷갈리면 그림을 그리거나 글을 써서 풀어 확인해봐야한다.

<br/>

### 5. 반복문
> 정해진 작업을 정해진 횟수만큼 수행하는 것

- while
    
    ```java
    while(조건) {
    내용
    }
    ```
    
    - 조건이 true면 내용을 실행한다. 내용을 실행한 뒤 조건으로 이동한다.
    - 조건이 false면 내용을 작업을 종료한다.

- do
    
    ```java
    do {
    내용
    } while(조건)
    
    ```
    
    - 내용을 먼저 실행하고 조건이 true 이면 내용을 실행한다. 조건이 false면 do~while을 종료한다. 특징은 조건을 따지지 않고 일단 실행함.

- for
    
    ```java
    for(초기값; 조건; 증감값) {
    내용
    }
    ```
    
    - 초기값을 지정한다.
    - 조건이 true이면 내용을 실행한다.
    - 증감값을 적용한다.
    - 조건으로 이동한다.
    - 조건이 false이면 for문을 중지한다.
        - 초기값은 if문 밖에서 적용할 수도 있음.
        - for문은 명확하게 횟수 컨트롤하기 용이함. 웬만해서 반복문 쓸 때 for문 사용하는 것 권장.

<br/>

## 6. 실습 2
### 6. 1. 1 ~ 100까지의 합계를 반복문으로 구하기.

```java		
// 1 ~ 100까지의 합계를 반복문으로 구하기.
// 누적 필요. 보관 공간 필요.
// 정답은 5050
int a = 0;
for(int i = 1; i < 101; i++) {
	a += i;
}
System.out.println(a);
```

### 6. 2.  1 ~ 100 사이의 2배수 또는 3배수를 출력하시오.

```java
for(int i = 1; i < 101; i++) {
    if(i % 2 == 0 || i % 3 == 0) {
        System.out.print(i + " ");
	}
}
```

### 6. 3. 피보나치 수열 출력 - 규칙을 찾는 것이 우선 / SWAP 로직과 유사함.

```java
int a = 1;
int b = 1;
int temp;
		
for(int i = 0; i < 10; i++) {
	System.out.println(a);
	temp = a + b;
	a = b;
	b = temp;
}
```

### 6. 4. 해당 숫자가 소수인지 아닌지 구별하시오.
1. 나누어지는 개수로 구별

```java
int cnt = 0;

for(int i = 1; i <= num; i++) {
    if(num % i == 0) {
        cnt++;
    }
}
if(cnt == 2) {
    System.out.println("소수입니다.");			
} else {
    System.out.println("소수가 아닙니다.");
}
```

2. 1과 자기 자신 숫자를 제외하고 나눠지면 소수가 아닌 경우로 구별

```java
boolean res = true;

for(int i = 2; i < num; i++) {
    if(num % i == 0) {
        res = false; // 1과 자기자신을 제외하고 나누어졌을 때 res에 false
        break; // 소수가 아니기 때문에 더 이상의 검증이 필요 없음.
    }
}
if(res) {
    System.out.println("소수입니다.");
} else {
    System.out.println("소수가 아닙니다.");
}
```