## 12장 - 스레드

- 핵심 키워드 : 프로세스, 멀티 스레드, 메인 스레드, 작업 스레드, 동기화 메소드
- 핵심 포인트 : 프로세스 내에서 작업을 처리하기 위한 코드 실행 흐름을 스레드라 한다.

- 프로세스(process)
    - 실행 중인 하나의 애플리케이션

- 스레드(thread)
    - 프로세스 내에서 작업을 처리하기 위한 코드 실행 흐름
    - 프로세스가 시작되면 메인 스레드가 생성되고, 프로세스를 시작하게 된다.

- 멀티 스레드(multi thread)
    - 하나의 프로세스 내에 두 개 이상의 스레드가 있을 경우 = 멀티 스레드
    - 각각의 스레드는 해당 작업을 처리하기 위해 별도의 코드 실행 흐름을 생성
    
- 메인 스레드 (main thread)
    - 모든 자바 애플리케이션은 메인 스레드가 main() 메소드 실행하면서 시작됨
    - main() 메소드의 첫 코드부터 아래로 순차적으로 실행
    - 필요에 따라 다른 스레드들 만들어 병렬로 실행
    - 프로세스 내에서 실행 중인 스레드가 하나라도 있으면 프로세스가 종료되지 않음.
        - main 스레드가 종료되었다고 해서 프로세스가 종료되는 것은 아님
    
- 작업 스레드 생성
    - 멀티 스레드로 실행하는 애플리케이션 개발하려면 몇 개의 작업을 병렬로 실행할지 우선 결정
    - 각 작업별 스레드 생성 방법
        - Thread 클래스로부터 직접 생성
        - Thread 하위(자식) 클래스로부터 생성

- Thread 클래스로부터 직접 생성
    
    ```java
    Thread thread = new Thread((Runnable target);
    ```
    
    - Runnable 구현 객체 = 스레드가 처리해야할 하나의 작업
        - 방법 1
            
            ```java
            class Task implements Runnable {
            	public void run() {
            	스레드가 실행할 코드;
            	}
            }
            ```
            
            ```java
            Runnable task = new Task();
            
            Thread thread = new Thread(task);
            ```
            
        - 방법 2
            
            ```java
            Thread thread = new Thread(new Runnable() { // 익명 구현 객체
            	public void run() {
            		스레드가 실행할 코드;
            	}
            }
            ```
            
    - start() 메소드 호출하면 작업 스레드 실행
        - `thread.start();`
        
    
- Thread 하위 클래스로부터 생성
    - Thread의 하위 클래스로 작업 스레드를 정의하면서 작업 내용을 포함
    - 작업 스레드 클래스 정의하는 법
        - 방법 1
            
            ```java
            public class WorkerThread extends Thread {
            	@Override // run() 메소드 재정의
            	public void run() {
            		스레드가 실행할 코드;
            	}
            }
            Thread thread = new WorkerThread();
            ```
            
        - 방법 2
            
            ```java
            Thread thread = new Thread() {
            	public void run() {
            		스레드가 실행할 코드;
            	}
            }
            ```
            
    - Strat() 메소드 호출하면 작업 스레드 실행
        - `thread.start();`