### 1. 변경에 유리한 코드(1) - 다형성, factory method

- 변경 사항이 발생했을 때, 변경 포인트를 줄이기 위함. 어떻게 하면 변경에 유리한 코드를 작성할 것인가에 대한 것

```java
// 1. 위 코드를 아래 코드처럼 바꿀 때
SprotsCar car = new SprotsCar();
Truck car = new Truck();

// 2. 이렇게 바꾸는 것이 변경에 유리함.
Car car = new SportsCar();
Car car = new Truck(); 
```

```java
// 3. 이렇게 또 바꾸는 것이 더 변경에 유리함.
Car car = getCar();

static Car getCar() {
	return new SportsCar();
}
```

### 2. 변경에 유리한 코드(2) - Map과 외부 파일

```java
Car car = getCar();
static Car getCar() throws Exception {
	// config.txt를 읽어서 Properties에 저장
	Properties p = new Properties();
	p.load(new FileReader("config.txt"));

	// 클래스 객체(설계도)를 얻어서
	Class clazz = Class.forName(p.getProperty("car"));
	return (Car)clazz.newInstance(); // 객체를 생성해서 반환
}
```

- Properties를 사용해서 외부에서 클래스 정보를 얻어오면 **변경 포인트를 줄일 수 있다.**
- 코드가 변경되면 테스트가 필수인데 코드가 변경되지 않는다는 것은 굉장한 이점. (코드 변경 최소화)
- OOP는 **변경에 유리한 코드를 작성하기 위해 나온 개념**.
- **분리**
    1. 변하는 것과 변하지 않는 것
    2. 관심사
    3. 중복코드

### 3. 객체 컨테이너(Application Context) 만들기

- 객체 컨테이너는 객체 저장소이다.
- 클래스 안에 Map을 가지고 있다. Map이 객체 저장소.
- Properties(String, String)에 있는 내용을 Map(String, Object)로 변환. 객체를 저장하기 위해서.

### 4. 자동 객체 등록하기 - Component Scanning

- class 앞에 @Component 를 붙이면 패키지 내에 @Component 붙은 클래스를 찾아서 객체 생성해서 map에 저장.
- guava 라이브러리를 이용해서 작성. 자바 리플렉션을 더 쉽게 도와주는 라이브러리.

### 5. 객체 찾기 - by Name, by Type

### 6. 객체를 자동 연결하기 - @Autowired

- 객체 저장소에 객체가 등록되어 있을 때, 수동으로 객체 참조를 연결해주는 것이 아니라 자동으로 하는 것이 @Autowired
- @Autowired 어노테이션이 붙어 있으면 객체 저장소를 훑어서 해당하는 객체를 연결해주는 것이다.
- @Autowired는 ByType으로 찾아서 연결해주는 것

### 7. 객체를 자동 연결하기 - @Resource

- @Resource는 ByName으로 찾아서 연결해주는 것
- value를 instanceOf로 찾는 것

# 02. Spring DI 활용하기 - 실습
### 1. 