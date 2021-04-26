# 코딩테스트 - Array , Tow pointers, Sliding window

## 12 . 멘토링

- 문제

  현수네 반 선생님은 반 학생들의 수학점수를 향상시키기 위해 멘토링 시스템을 만들려고 합니다.

  멘토링은 멘토(도와주는 학생)와 멘티(도움을 받는 학생)가 한 짝이 되어 멘토가 멘티의 수학공부를 도와주는 것입니다.

  선생님은 M번의 수학테스트 등수를 가지고 멘토와 멘티를 정합니다.

  만약 A학생이 멘토이고, B학생이 멘티가 되는 짝이 되었다면, A학생은 M번의 수학테스트에서 모두 B학생보다 등수가 앞서야 합니다.

  M번의 수학성적이 주어지면 멘토와 멘티가 되는 짝을 만들 수 있는 경우가 총 몇 가지 인지 출력하는 프로그램을 작성하세요.

- 생각

  - 2차열 배열 행은 테스트 횟수 열은 등수로 한다
  - 4중 for문을 사용

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Sol12 {
  	public static int solution(int n,int m,int[][] a) {
  		int answer = 0;
  		for(int i=1;i<=n;i++) {
  			for(int j=1;j<=n;j++) { // 모든 학생의 경우의수
  				int cnt = 0;
  				for(int k=0;k<m;k++) {
  					int pi = 0, pj = 0;
  					for(int s=0;s<n;s++) { //각 테스트
  						if(a[k][s] == i) {
  							pi = s;
  						}
  						if(a[k][s] == j) {
  							pj = s;
  						}
  					}
  					if(pi < pj) {
  						cnt ++;
  					}
  				}
  				if(cnt == m) {
  					answer ++;
  				}
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int m = kb.nextInt();
  		int[][] a = new int[n][n];
  		for(int i=0;i<m;i++) {
  			for(int j=0;j<n;j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n,m, a));
  	}
  }
  
  ```

  

## 1.두 배열 합치기

- 문제

  오름차순으로 정렬이 된 두 배열이 주어지면 두 배열을 오름차순으로 합쳐 출력하는 프로그램을 작성하세요.

- 생각

  - 두 배열을 합치고 그리고 나서 sort함수를 사용해서 정렬하면 좋을것같다.
  - 

- 내 풀이

  ```
  package sec03;
  
  import java.util.Arrays;
  import java.util.Scanner;
  
  public class Ex01 {
  	public static int[] solution(int[] arr1 , int[] arr2) {
  		int[] answer = new int[arr1.length+arr2.length];
  		System.arraycopy(arr1, 0, answer, 0, arr1.length);
  		System.arraycopy(arr2, 0, answer, arr1.length, arr2.length);
  		Arrays.sort(answer);
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt();
  		int[] arr1 = new int[n];
  		for(int i=0;i<n;i++) {
  			arr1[i] = kb.nextInt();
  		}
  		
  		int m = kb.nextInt();
  		int[] arr2 = new int[m];
  		for(int i=0;i<m;i++) {
  			arr2[i] = kb.nextInt();
  		}
  		
  		for(int i : solution(arr1, arr2)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

- 풀이

  - 두 개의 포인터를 이용해서 문제를 풀어야한다.!!

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  public class Sol01 {
  	public static ArrayList<Integer> solution(int n ,int  m, int[] a , int[] b) {
  		ArrayList<Integer> answer = new ArrayList<Integer>();
  		int p1=0, p2=0;
  		while(p1<n && p2<m) {
  			if(a[p1] < b[p2]) {
  				answer.add(a[p1]);
  				p1++;
  			}else {
  				answer.add(b[p2]);
  				p2++;
  			}
  		}
  		while(p1<n) { // p1이 남아있으면
  			answer.add(a[p1]);
  			p1++;
  		}
  		while(p2<m){//p2가 남아있으면
  			answer.add(b[p2]);
  			p2++;
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt();
  		int[] a = new int[n];
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		
  		int m = kb.nextInt();
  		int[] b = new int[m];
  		for(int i=0;i<m;i++) {
  			b[i] = kb.nextInt();
  		}
  		
  		for(int i : solution(n,m,a,b)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

  