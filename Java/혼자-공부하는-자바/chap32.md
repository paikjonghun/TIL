## 64강 - java.util 패키지

- 핵심 키워드 : Date 클래스, Calendar 클래스
- 핵심 포인트 : java.util 패키지는 날짜 정보를 제공하는 유용한 API를 포함하고 있다.

- Date 클래스 : 날짜와 시간 정보를 저장하는 클래스
- Calendar 클래스 : 운영체제의 날짜와 시간을 얻을 때 사용

## Date 클래스

- 날짜를 표현하는 클래스
- Date는 객체 간 날짜 정보 주고받을 때 매개 변수나 리턴 타입으로 주로 사용
    
    ```java
    Date now = new Date();
    ```
    
- 원하는 날짜 형식의 문자열 얻기 위해 java.text 패키지의 SimpleDataFormat 클래스와 함께 사용
    
    ```java
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 DD일 hh시 mm분 ss초");
    ```
    
- format() 메소드 호출
    
    ```java
    String strNow = sdf.format(now);
    ```
    

## Calendar 클래스

- 달력을 표현한 클래스로 운영체제의 날짜 및 시간 기준으로 다양한 정보를 얻을 수 있음
- 추상 클래스이므로 new 연산자 사용하여 인스턴스 생성 불가
- getInstance() 메소드 이용하여 Calendar 하위 객체 얻을 수 있음

```java
Calendar now = Calendar.getInstance();

int year = now.get(Calendar.YEAR); // 연도를 리턴
int month = now.get(Calendar.MONTH); // 월을 리턴
int day = now.get(Calendar.DAY_OF_MONTH); // 일을 리턴
int week = now.get(Calendar.DAY_OF_WEEK); // 요일을 리턴
int amPm = now.get(Calendar.AM_PM); // 오전 / 오후를 리턴
int hour = now.get(Calendar.HOUR); // 시를 리턴
int minute = now.get(Calendar.MINUTE); // 분을 리턴
int second = now.get(Calendar.SECOND); / 초를 리턴
```

- 핵심 포인트
    - Date 클래스 :
        - 날짜를 표현하는 클래스,
        - 객체간 날짜 정보 주고받을 때 매개 변수나 리턴 타입으로 주로 사용.
    - Calendar 클래스 :
        - 달력을 표현하는 클래스로 운영체제의 날짜 및 시간 기준으로 다양한 정보를 얻을 수 있음.
        - 추상 클래스이므로 new 연산자 사용한 인스턴스 생성이 불가능.
        - Calendar 클래스의 정적 메소드인 getInstance() 메소드 이용하여 Calendar 하의 객체 얻을 수 있음