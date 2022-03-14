## 배열이란?

- 배열은 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것

```java
int[] score = new int[5];
```

## 배열의 선언과 생성

- 배열의 선언 - 배열을 다루기 위한 참조변수의 선언
    - 선언 방법
        - `타입[] 변수이름;` - 자바 스타일. 배열의 기호를 타입의 일부로 봄.
        - `타입 변수이름[];` - C언어 스타일
    - 배열 생성
        - `변수이름 = new 타입[길이];`
            
            ```java
            int[] score; // int타입의 배열을 다루기 위한 참조변수 score 선언
            score = new int[5]; // int타입의 값 5개를 저장할 수 있는 배열 생성
            ```
            
    - 배열의 선언과 생성을 동시에 하려면
        
        ```java
        int[] score = new int[5];
        ```
        

## 배열의 인덱스

- 배열의 인덱스 - 각 요소에 자동으로 붙는 번호
    - 인덱스(index)의 범위는 0부터 ‘배열길이-1’까지
    
    ```java
    int[] score = new int[5];
    score[3] = 100;
    int value = score[3];
    // value == 100
    ```

## 배열의 길이

- `배열이름.length` - 배열의 길이(int형 상수)
    
    ```java
    int[] arr = new int[5]; // 길이가 5인 int배열
    int tmp = arr.length; // arr.length의 값은 5이고 tmp에 5가 저장된다.
    ```
    
- 배열은 한번 생성하면 실행하는 동안 그 길이를 바꿀 수 없다. 그래서 배열의 길이는 상수

```java
int[] arr = new int[5]; // 길이가 5인 int 배열 arr을 생성
System.out.println("arr.length=" + arr.length);

for(i = 0; i < arr.length; i++) {
	System.out.println("arr["+i+"]="+arr[i]);
}
```

- 배열의 인덱스 범위를 벗어나는 에러를 조심해야한다.
    - 그래서 for문에서 arr.length를 활용하는 것이 좋다.

## 배열의 초기화

- 배열의 각 요소에 처음으로 값을 저장하는 것
    
    ```java
    int[] score = new int[5]; // 길이가 5인 int 배열을 생성
    score[0] = 50; // 각 요소에 직접 값을 한다.
    score[1] = 60;
    score[2] = 70;
    score[3] = 80;
    score[4] = 90;
    ```
    
- 일일이 초기화를 하는 것이 아니라 처음에 한번에 초기화 하는 방법
    
    ```java
    int[] scroe = {50, 60, 70, 80, 90}; // new int[] 를 생략할 수 있다.
    
    int[] score;
    score = {50, 60, 70, 80, 90]; // 에러 발생. new int[]를 생략할 수 없음
    scroe = new int[]{50, 60, 70, 80, 90}; // OK. 선언과 초기화를 나눠쓸 때 이렇게 사용해야 함.
    ```

## 배열의 출력

```java
int[] iArr = {100, 95, 80, 70, 60};

// 배열을 가리키는 참조변수 iArr의 값을 출력한다.
System.out.println(iArr); // [I@14318bb와 같은 형식의 문자열이 출력된다.
// [ <- 배열을 의미. I <- Integer를 의미. 그 다음은 주소값.
```

- 배열 이름을 출력하면 배열이 출력되지 않는다.
    - 예외
        
        ```java
        char[] chArr = {'a', 'b', 'c', 'd'};
        System.out.println(chArr); // abcd가 출력된다.
        ```
        

- 배열의 요소를 순서대로 하나씩 출력
    
    ```java
    for(int i=0; i < iArr.length; i++) { // 배열의 요소를 순서대로 하나씩 출력
    	System.out.println(iArr[i]);
    }
    ```
    
    ```java
    // 배열 출력하는 두 번째 방법.
    // 배열의 iArr 요소를 출력한다. 
    System.out.println(Arrays.toString(iArr));
    ```

## 배열의 활용

- 총합과 평균 - 배열의 모든 요소를 더해서 총합과 평균을 구한다.

```java
int sum = 0; // 총합을 저장하기 위한 변수
float average = 0f; // 평균을 저장하기 위한 변수

int[] score = {100, 88, 100, 100, 90};

for(int i = 0; i < score.length; i++) {
	sum += scroe[i];
}
average = sum / (float)score.length; // 계산 결과를 float타입으로 얻으려 형변환

System.out.println("총합 :" + sum);
System.out.println("평균 :" + average);
```

## 배열의 활용 (2)

- 최대값과 최소값 - 배열의 요소 중에서 제일 큰 값과 제일 작은 값을 찾는다.

```java
int[] score = {79, 88, 91, 33, 100, 55, 95};

int max = scroe[0]; // 배열의 첫 번째 값으로 최대값을 초기화한다.
int min = score[0]; // 배열의 첫 번째 값으로 최소값을 초기화한다.

for(int i = 1; i < score.length; i++) {
	if(score[i] > max) {
		max = score[i];
	} else if(score[i] < min) {
		min = score[i];
	}
}

System.out.println("최대값 :" + max);
System.out.println("최소값 :" + min);
```

## 배열의 활용 (3)

- 섞기(shuffle) - 배열의 요소의 순서를 반복해서 바꾼다. (숫자 섞기, 로또번호 생성)

```java
int[] numArr = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
System.out.println(Arrays.toString(numArr));

for(int i = 0; i < 100; i++) {
	int n = (int)(Math.random() * 10); // 0~9 중의 한 값을 임의로 얻는다.
	int tmp = numArr[0];
	numArr[0] = numArr[n];
	numArr[n] = tmp;
}

System.out.println(Arrays.toString(numArr));
```

## String 배열의 선언과 생성

```java
String[] name = new String[3]; // 3개의 문자열을 담을 수 있는 배열을 생성한다.
```

- 가위, 바위, 보 랜덤 출력

```java
String[] strArr = {"가위", "바위", "보"};

int tmp = (int)(Math.random()*3);

System.out.println(strArr[tmp]);
```

## 커맨드 라인을 통해 입력 받기

- 커맨드 라인에 입력한 값이 문자열 배열에 담겨서 전달된다.

```java
System.out.println("매개변수의 개수 :" + args.length);
for(int i = 0; i < args.length; i++) {
	System.out.println("args[" + i + "] = \"" + args[i] + "\"");
}

// 결과 :
// 매개변수의 개수 : 3
// args[0] = "abc"
// args[1] = "123"
// args[2] = "Hello World"
```

- 매개변수 이클립스에서 넣기
    - Run Configurations 클릭 → Arguments 탭 → Program arguments 에 입력.

- cmd에서 실행
    - → `cd 클래스경로`
    - → `java 클래스이름.class abc 123 “Hello World”`

## 2차원 배열

- 테이블 형태의 데이터를 저장하기 위한 배열

| 국어 | 영어 | 수학 |
| --- | --- | --- |
| 100 | 30 | 100 |
| 20 | 40 | 20 |

```java
int[][] score = new int[4][3]; // 4행 3열의 2차원 배열을 생성한다.
// intger 값 12개 공간이 생성됨.
```

- 2차원 배열은 인덱스 2개 사용.
    - 1행 1열에 100을 저장하려면
        - `score[0][0] = 100;`

## 2차원 배열의 초기화

```java
int[][] arr = {{1, 2, 3}, {4, 5, 6}};

int[][] arr = {
								{1, 2, 3},
								{4, 5, 6}
							};
```

- 2차원 배열은 1차원 배열의 배열

## 2차원 배열 예제

```java
int[][] score = {{100, 100, 100}, {20, 20, 20}, {30, 30, 30}, {40, 40, 40}};
int sum = 0;

for(int i = 0; i < scroe.length; i++) {
	for(int j = 0; j < score[i].length; j++) {
		System.out.printf("score[%d][%d]=%d%n", i, j, score[i][j]);
		sum += score[i][j];
	}
}
System.out.println("sum=" + sum);
```

## String 클래스

1. String 클래스는 char[]와 메소드(기능)를 결합한 것
    1. String 클래스 = char[] + 메소드(기능)
2. String 클래스는 내용을 변경할 수 없다.(read only)

## String 클래스의 주요 메소드

- char charAt(int index)
    - 문자열에서 해당 위치(index)에 있는 문자를 반환한다.
- int length()
    - 문자열의 길이를 반환한다.
- String substring(int from, int to)
    - 문자열에서 해당 범위(from~to)의 문자열을 반환한다.(to는 포함 안 됨)
- boolean equals(Object obj)
    - 문자열의 내용이 같은지 확인한다. 같으면 결과는 true. 다르면 false.
- char[] toCharArray()
    - 문자열을 문자배열(char[])로 변환해서 반환한다.

## Arrays로 배열 다루기 - Arrays는 클래스

- 배열의 비교와 출력 - equals(), toString()

```java
int[] arr = {0, 1, 2, 3, 4];
int[][] arr2D = {{11, 12}, {21, 22}};

System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
System.out.println(Arrays.deepToString(arr2D)); // [[11, 12], [21, 22]]
```

```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] Str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Arrays.equals(str2D, str2D2)); // false
System.out.pirntln(Arrays.deepEquals(str2D, str2D2)); // true
```

- 배열의 복사 - copyOf(), copyOfRange()

```java
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length); // arr2=[0,1,2,3,4]
int[] arr3 = Arrays.copyOf(arr, 3); // arr3=[0,1,2]
int[] arr4 = Arrays.copyOf(arr, 7); // arr4=[0,1,2,3,4,0,0]

int[] arr5 = Arrays.copyOfRange(arr, 2, 4); // arr5=[2,3] 4는 불포함
int[] arr6 = Arrays.copyOfRange(arr, 0, 7); // arr6=[0,1,2,3,4,0,0]
```

- 배열의 정렬 - sort()

```java
int[] arr = {3, 2, 0, 1, 4};
Arrays.sort(arr); // 오름차순 정렬
System.out.println(Arrays.toString(arr)); // [0,1,2,3,4]
```

