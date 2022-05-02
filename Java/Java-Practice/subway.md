### 지하철 게임 만들기
---
```java
import java.util.Scanner;

public class Subway {

	public static void main(String[] args) {
		System.out.println("*****************************");
		System.out.println("*   지하철에 오신 것을 환영합니다. *");
		System.out.println("*   원하는 서비스를 입력하세요.    *");
		System.out.println("*****************************");
		
		Scanner sc = new Scanner(System.in);
		int stat = 0;
		int cnt = 1;
		int goal = 0;
		String[][] train = {{"[ ]", "[ ]", "[ ]", "[ ]"}, {"[ ]", "[ ]", "[ ]", "[ ]"}, {"[ ]", "[ ]", "[ ]", "[ ]"}, {"[ ]", "[ ]", "[ ]", "[ ]"}};
		String[] station = {"장승배기", "연신내", "강남", "양재", "평택"};
		
		while(true) {
			System.out.println("1. [이동] 2. [탑승] 3. [현재 상태 보기] 9. [종료]");
			System.out.print("입력 : ");
			
			String input = sc.nextLine();
			if(input.equals("1")) {
				stat += cnt;
				if(stat == 4 || stat == 0) {
					cnt *= -1;
				}
				
				System.out.println("* 이번 역은 [" + station[stat] + "] 입니다. *");
				
				for(int i = 0; i < train.length; i++) {
					for(int j = 0; j < train[i].length; j++) {
						if(train[i][j].equals("[" + station[stat] + "]")) {
							train[i][j] = "[ ]";
						}
					}
				}
			} else if(input.equals("2")) {
				for(int i = 0; i < 5; i++) {
					if(stat == i) {
						System.out.println("* 현재 역은 [" + station[i] + "] 입니다. *");
					}
				}
				System.out.println("어느 역까지 가시겠습니까?");
				System.out.println("1. [장승배기] 2. [연신내] 3. [강남] 4. [양재] 5. [평택]");
				System.out.print("입력 : ");
				
				input = sc.nextLine();
				
				for(int i = 0; i < 5; i++) {
					if(input.equals(Integer.toString(i+1))) {
						goal = i;
						System.out.println("["+ station[i] + "] 을(를) 선택하셨습니다.");
					} else {
						while(Integer.parseInt(input) > 5 || Integer.parseInt(input) < 0) {
							System.out.println("잘못 누르셨습니다. 다시 선택해주세요.");
							System.out.println("1. [장승배기] 2. [연신내] 3. [강남] 4. [양재] 5. [평택]");
							input = sc.nextLine();
						}
					}
				}
				
				for(int i = 0; i < train.length; i++) {
					System.out.print((i + 1) + "호 차 : ");
					for(int j = 0; j < train[i].length; j++) {
						System.out.print(train[i][j] + " ");
					}
					System.out.println();
				}
				
				System.out.println("몇 호차에 탑승하시겠습니까?"); // 현재 역은 -> 탑승하고 싶은 칸? -> 내리고 싶은 칸?
				System.out.print("입력 : ");
				
				input = sc.nextLine();
				
				if(input.equals("1")) {
					if(train[0][3] != "[ ]") {
						System.out.println("*** 해당 호 차는 현재 꽉찼습니다. ***");
						System.out.println("*** 다시 선택해 주세요. ***");
					}
					for(int i = 0; i < train[0].length; i++) {
						if(train[0][i] == "[ ]") {
							train[0][i] = "[" + station[goal] + "]";
							System.out.println("* 1호 차 탑승 완료 *");
							break;
						}
					}
				} else if(input.equals("2")) {
					if(train[1][3] != "[ ]") {
						System.out.println("*** 해당 호 차는 현재 꽉찼습니다. ***");
						System.out.println("*** 다시 선택해 주세요. ***");
					}
					for(int i = 0; i < train[1].length; i++) {
						if(train[1][i] == "[ ]") {
							train[1][i] = "[" + station[goal] + "]";
							System.out.println("* 2호 차 탑승 완료 *");
							break;
						} 
					}
				} else if(input.equals("3")) {
					if(train[2][3] != "[ ]") {
						System.out.println("*** 해당 호 차는 현재 꽉찼습니다. ***");
						System.out.println("*** 다시 선택해 주세요. ***");
					}
					for(int i = 0; i < train[2].length; i++) {
						if(train[2][i] == "[ ]") {
							train[2][i] = "[" + station[goal] + "]";
							System.out.println("* 3호 차 탑승 완료 *");
							break;
						}
					}
				} else if(input.equals("4")) {
					if(train[3][3] != "[ ]") {
						System.out.println("*** 해당 호 차는 현재 꽉찼습니다. ***");
						System.out.println("*** 다시 선택해 주세요. ***");
					}
					for(int i = 0; i < train[3].length; i++) {
						if(train[3][i] == "[ ]") {
							train[3][i] = "[" + station[goal] + "]";
							System.out.println("* 4호 차 탑승 완료 *");
							break;
						}
					}
				} else {
					System.out.println("해당 호 차는 존재하지 않습니다.");
				}
			} else if(input.equals("3")) {
				System.out.println("* 현재 정차역 : [" + station[stat] + "]");
				System.out.println("* 객실 현황 : "); // 현재 역 + 열차 4칸 현황
				for(int i = 0; i < train.length; i++) {
					System.out.print((i + 1) + "호 차 : ");
					for(int j = 0; j < train[i].length; j++) {
						System.out.print(train[i][j] + " ");	
					}
					System.out.println();
				}
			} else if(input.equals("9")) {
				System.out.println("*** 열차 운행을 종료합니다. ***");
				break;
			} else {
				System.out.println("*** 잘못 누르셨습니다. 다시 선택해 주세요. ***");
			}
		} 

	}
	
}
```