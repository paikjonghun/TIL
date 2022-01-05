### Java 숫자 야구 게임 만들기 - 다른 풀이 해석하기
> 숫자 값을 서로 비교하는 방법을 다룸

```java
public class NumBaseballController {
	public static void main(String[] args) {
		NumBaseballService numbaseballservice = new NumBaseballService();

		numbaseballservice.run();
	}
}

```




```java
import java.util.Arrays;
import java.util.Scanner;

public class NumBaseballService {
	Scanner scanner = new Scanner(System.in); // 스캐너 변수 선언
	int ranNum[] = new int[3] ; // 랜덤 숫자 3개를 담을 정수 배열 선언
	int inputNum[] = new int[3] ; // 입력 숫자 3개를 담을 정수 배열 선언
	int count = 1; // 몇 회인지 저장하는 정수 변수 선언
	boolean flag = true; // while문에 사용할 논리 변수 선언 

	public void run() { // 컨트롤러에서 사용할 run 메소드 선언
		System.out.println("-----------------");
		System.out.println("---- 숫자 야구 ---- ");
		System.out.println("-----------------");
		
		RandomNum(); // 랜덤 숫자 3개를 뽑는 RandomNum 메소드
		
		while(flag) { 
			InsertNum(); // 숫자 3개를 입력하는 InsertNum 메소드
		}
	}

	
// 랜덤 숫자 뽑기
	public void RandomNum() {
		for (int i = 0 ; i < 3 ; i ++) {
			// 랜덤 숫자를 뽑고 ranNum 배열, i번째에 넣기 
			ranNum[i] = (int)Math.floor(Math.random() * 9) + 1 ;
			
			for(int j = 0 ; j < i ; j ++) {
				// ranNum i번째와 ranNum j번째가 같으면
				if (ranNum[i] == ranNum[j]) {
					i--; //
					break;
				}
			}
	
		}		
	//	System.out.println(ranNum[0] + " " +  ranNum[1] + " " + ranNum[2]);		
	}
	
	
/** 숫자 세개 입력하기
 ***************************************************************************
 ***************************************************************************
 */	
	public void InsertNum() {
		System.out.println();
		System.out.println("1~9 중 서로 다른 숫자 세 개를 입력하세요!! ex) 1 2 3    [ 리셋:0 / 종료:99 ]");
		
		String temp = scanner.nextLine();
		String[] tempArr = temp.split(" ");
		
		//리셋
		if (tempArr[0].equals("0")) {
			Reset();
		}
		//종료
		else if (tempArr[0].equals("99")) {
			System.out.println("종료합니다.");
			flag = false;
		}
		else {
			if(tempArr.length != 3) {
				System.out.println("숫자를 3개 입력해 주세요.");
			} else {
				boolean range = true;
				
				for(int i = 0 ; i < tempArr.length ; i++) {
					inputNum[i] = Integer.parseInt(tempArr[i]);
					if(inputNum[i] > 9 || inputNum[i] < 1) {
						range = false;
						break;
					}
				}
				
				boolean check = true;
				//같은 숫자가 있는지 검사
				for(int i = 0 ; i < 2 ; i ++) {
					for (int j = i + 1 ; j < 3 ; j ++) {
						if(inputNum[i] == inputNum[j]) {
							check = false;
						}
					}
				}
				
				if(!range) {
					System.out.println("범위는 1 ~ 9입니다.");
				} else if(!check) {
					System.out.println("같은 숫자는 입력 할 수 없습니다.");
				} else {
					System.out.print(count+"회 - 입력값: " + inputNum[0] + " " + inputNum[1] + " "+ inputNum[2] + " ");
					count++;
					
					Result();
				}
			}
		}
	}
	
	
/** 게임 결과 출력
 ***************************************************************************
 ***************************************************************************
 */	
	public void Result() {
		int strike = 0 ;
		int ball = 0 ;
		
		for(int i = 0; i < 2 ; i++) {
			for(int j = i + 1; j < 3; j++) {
				if(ranNum[i] == inputNum[j] && i == j) {
					strike++;
				}else if (ranNum[i] == inputNum[j]) {
					ball++;
				}
			}
		}
		
		int out = 3 - strike - ball;;
		
		// 결과 출력
		System.out.println(" [" + strike + "S " + ball + "B " + out + "O]");
		
		//스트라이크 세개면
		if(strike ==3) {
			Victory();
		} else if(count == 10) {
			Over();
		}
		
	} //class end
	
	

/** 승리
 ***************************************************************************
 ***************************************************************************
 */	
	public void Victory() {
		System.out.println("      숫자: " + ranNum[0] + " " +  ranNum[1] + " " + ranNum[2]);	
		System.out.println("-----------------");
		System.out.println("!!! 승리하였습니다 !!!");
		System.out.println("-----------------");
		Reset();
	}

/** 리셋
 ***************************************************************************
 ***************************************************************************
 */
	public void Reset() {
		count = 1;
		flag = true;
		
		for (int i = 0 ; i < 3 ; i ++) {
			ranNum[i] = -1;
			inputNum[i] = -1;
		}

		System.out.println("-----------------");
		System.out.println("---- 숫자 야구 ---- ");
		System.out.println("-----------------");
		
		RandomNum();
	}
	
	
/** 종료
 ***************************************************************************
 ***************************************************************************
 */	
	public void Over() {
		System.out.println();
		System.out.println("-----------------");
		System.out.println("--- Game Over ---");
		System.out.println("-----------------");
		Reset();
	}

} //class end
```