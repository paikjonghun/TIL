### 자바로 숫자 야구 만들기

---

```java
import java.util.Scanner;

public class NumberBaseball0104 {

	public static void main(String[] args) {
		System.out.println("* 숫자 야구 게임 *");
		int[] r_numb = new int[3];
		
		for(int i = 0; i < 3; i++) { // 중복 없는 랜덤 숫자 3개 뽑기
			r_numb[i] = (int)(Math.floor(Math.random() * 9) + 1);
			for(int j = 0; j < i; j++) {
				if(r_numb[i] == r_numb[j]) {
					i--;
					break;
				}
			}
		}		
		
		Scanner sc = new Scanner(System.in);	
		boolean f = true;
		int[] input = new int[3];
		int r = 1;
		
		while(f) {
			int s = 0;
			int b = 0;
			int o = 0;
			System.out.println("[" + r + "]회 차");
			System.out.println("숫자 세 개를 입력하세요.");
			System.out.print("입력 : ");
			
			String in = sc.nextLine();
			input[0] = Integer.parseInt(in.substring(0, 1));
			input[1] = Integer.parseInt(in.substring(1, 2));
			input[2] = Integer.parseInt(in.substring(2, 3));
			
			
			for(int i = 0; i < 2; i++) {
				for(int j = i + 1; j < 3; j++) {
					if(input[i] == input[j]) {
						f = false;
						System.out.println("중복된 숫자는 입력할 수 없습니다.");
						break;
					}
				}
				if(f == false) {
					break;
				}
			}
			
			if(f != false) { // 중복된 숫자가 없을 시
				for(int i = 0; i < 3; i++) {
					for(int j = 0; j < 3; j++) {
						if(input[i] == r_numb[j]) {
							if(input[i] == r_numb[i]) {
								s++;
							} else {
								b++;
							}
						}
					}
				}
				o = 3 - (s + b);
				
				System.out.println("결과 : " + s + "S " + b + "B " + o + "O ");
				r++;
				
				if(s == 3) {
					f = false;
					System.out.println("승리!");
				}
				if(r == 10) {
					f = false;
					System.out.println("패배!");
				}
			}
		}
				

	}

}
```