# 코딩테스트 - Array3

## 11. 같은 반 학생

- 생각

  - 같은 반이면 i번 학생의 학년 j번 학생의 학년을 비교

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Ex11 {
  	public static int solution(int n, int[][] a) {
  		int answer = 0 , max = Integer.MIN_VALUE;
  		for(int i=1; i<=n; i++) { //1번학생부터 n번학생까지 학생까지
  			int cnt =0 ;
  			for(int j=1;j<=n;j++) { // 1번 학생부터 n번 학생까지
  				for(int k=1;k<=5;k++) { //1학년부터 5학년 까지
  					if(a[i][k] == a[j][k]) { // i번학생의 k학년의 값과 j번학생의 k학년의 값이 같은지
  						cnt ++ ;
  						break;
  					}
  				}
  			}
  			if(cnt > max) {
  				max = cnt ; 
  				answer = i;
  			}
  		}
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[][] a = new int[n+1][6];
  		for (int i = 1; i <= n; i++) {
  			for (int j = 1; j < 6; j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n, a));
  	}
  }
  
  ```

  

