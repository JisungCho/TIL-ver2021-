# 코딩테스트  - Array2

## 5. 소수

- 문제

  자연수 N이 입력되면 1부터 N까지의 소수의 개수를 출력하는 프로그램을 작성하세요.

  만약 20이 입력되면 1부터 20까지의 소수는 2, 3, 5, 7, 11, 13, 17, 19로 총 8개입니다.

- 개념

  - 에라토스테네스 체

    예를 들면1부터 50까지의 수를 배열한 다음 1은 소수가 아니므로 제외하고 2는 소수이므로 남겨둔 다음 2보다 큰 2의 배수인 4,6,8… 등을 제거한다．2다음의 수 3이 소수이므로 남겨두고 3보다 큰 3의 배수인 6,9,12… 등을 제거한다．다음에는 5가 소수이므로 남겨두고 5보다 큰 5의 배수인 10,15,20… 등을 제거한다．이러한 방법을 연속적으로 적용하면 소수만 남게 된다

- 풀이

  ```java
  package sec02;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *  생각 : 숫자가 입력되면 1부터 N까지 반복문을 돌면서
   * 다시 그 안에서 1부터 현재 반복문을 돌고 있는 숫자(i)까지 나누면서 
   * 나누어 떨어지는 갯수가 2이상이면 소수아님 
   *
   */
  public class Ex05 {
  	public static int solution(int n) {
  		int answer = 0;
  		int[] ch = new int[n+1];
  		for(int i=2;i<=n;i++) {
  			if(ch[i] == 0) {
  				answer++;
  				for(int j=i;j<=n;j=j+i) {
  					ch[j] = 1;
  				}
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		System.out.println(solution(n));
  	}
  }
  
  ```

## 6. 뒤짚은 소수

- 문제

  N개의 자연수가 입력되면 각 자연수를 뒤집은 후 그 뒤집은 수가 소수이면 그 소수를 출력하는 프로그램을 작성하세요.

  예를 들어 32를 뒤집으면 23이고, 23은 소수이다. 그러면 23을 출력한다. 단 910를 뒤집으면 19로 숫자화 해야 한다.

  첫 자리부터의 연속된 0은 무시한다.

- 개념

  - 일단 숫자를 뒤짚어 놓고 판별

- 내 풀이

  ```java
  package sec02;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *  
   *
   */
  public class Ex06 {
  	public static ArrayList<Integer> solution(int n,int[] arr) {
  		ArrayList<Integer> answer = new ArrayList<>();
  			for(int i=0;i<n;i++) {
  				int number = arr[i];
  				int reverse = 0;
  				int count =0;
  				while(number != 0) {
  					int remainder = number % 10;
  					reverse = reverse * 10 + remainder;
  					number = number / 10;
  				}
  				for(int j=1;j<=reverse;j++) {
  					if(reverse% j == 0) {
  						count ++;
  					}
  				}
  				if(count == 2) {
  					answer.add(reverse);
  				}
  			}
  			return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i =0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		for(int i : solution(n,arr)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

- 풀이

  ```java
  package sec02;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *  생각 : 숫자가 입력되면 1부터 N까지 반복문을 돌면서
   * 다시 그 안에서 1부터 현재 반복문을 돌고 있는 숫자(i)까지 나누면서 
   * 나누어 떨어지는 갯수가 2이상이면 소수아님 
   *
   */
  public class Sol06 {
  	public static boolean isPrime(int num) {
  		if(num == 1) {
  			return false;
  		}
  		for(int i = 2; i<num;i++) {
  			if(num % i == 0) { // 2부터 자기자신 전까지 약수가 있으면 소수가 아님
  				return false;
  			}
  		}
  		return true;
  	}
  	public static ArrayList<Integer> solution(int n,int[] arr) {
  		ArrayList<Integer> answer = new ArrayList<>();
  		for(int i=0;i<n;i++) {
  			int tmp = arr[i];
  			int res = 0;
  			while(tmp > 0) {
  				int t = tmp % 10;
  				res = res*10 + t;
  				tmp = tmp / 10;
  			}
  			if(isPrime(res)) {
  				answer.add(res);
  			}
  		}
  		
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i =0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		for(int i : solution(n,arr)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```


## 7. 점수계산

- 내 풀이 == 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  /**
   * @author jisun
   * 해당 배열인덱스를 조회해서 1이면 num을 1증가
   * 해당 배열인덱스를 조회해서 0이면 num은 0으로 초기화
   * 
   */
  public class Ex07 {
  	public static int solution(int n,int[] a) {
  		int num = 0;
  		int score = 0;
  		for(int i=0;i<n;i++) {
  			if(a[i] == 1) {
  				num++;
  				score += num;
  			}else {
  				num = 0;
  			}
  		}
  		return score;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] a = new int[n];
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		System.out.println(solution(n,a));
  	}
  }
  
  ```

## 8. 등수구하기

- 현재 인덱스(i)의 값을 배열의 처음부터 끝까지 조사하면서 현재 인덱스의 값(i)보다 조사한 배열의 인덱스의 값(j)이 더 크면 rate를 증가시켜줌

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	이중 for문을 돌면서
   *	현재 인덱스의 값과 배열의 처음부터 끝을 조사하면서 현재 인덱스의 값보다
   * 조사한 배열의 인덱스의 값이 더 크면 rate를 증가시켜줌
   */
  public class Ex08 {
  	public static int[] solution(int n,int[] a) {
  		int[] answer = new int[n];
  		for(int i=0;i<n;i++) {
  			int rate = 1;
  			for(int j=0;j<n;j++) {
  				if(a[i] < a[j]) {
  					rate++;
  				}
  			}
  			answer[i] = rate;
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
  		for(int i : solution(n, a)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

## 9. 격자판 최대합

- 내 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  
  /**
   * @author jisun
   * 행과 열을 비교하교 대각선을 따로 비교
   */
  public class Ex09 {
  	public static int solution(int n,int[][] a) {
  		int answer = Integer.MIN_VALUE;
  		int sum1;
  		int sum2;
  		int sum3 = 0;
  		int sum4 = 0;
  		//행중에 최대값
  		for(int i =0;i<n;i++) {
  			sum1 = sum2  = 0;
  			for(int j=0;j<n;j++) {
  				sum1 += a[i][j];
  				sum2 += a[j][i];
  				if(i == j) {
  					sum3 += a[i][j];
  				}
  				if(i+j == n-1) {
  					sum4 += a[i][j];
  				}
  			}
  			answer = Math.max(answer, sum1);
  			answer = Math.max(answer, sum2);
  		}
  		answer = Math.max(answer, sum3);
  		answer = Math.max(answer, sum4);
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[][] a = new int[n][n];
  		for(int i=0;i<n;i++) {
  			for(int j=0;j<n;j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n, a));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  
  
  public class Sol09 {
  	public static int solution(int n,int[][] a) {
  		int answer = Integer.MIN_VALUE;
  		int sum1; //행의 합
  		int sum2;  // 열의 합
  		for(int i=0;i<n;i++) {
  			sum1 = sum2 = 0;
  			for(int j=0;j<n;j++) {
  				sum1 += a[i][j];// i행의 합
  				sum2 += a[j][i];// i열의 합
  			}
  			answer = Math.max(answer, sum1);
  			answer = Math.max(answer, sum2);
  		}
  		//두 대각선의 합
  		sum1 = sum2 = 0;
  		for(int i=0;i<n;i++) {
  			sum1 += a[i][i]; // 좌->우 대각선
  			sum2 += a[i][n-i-1];
  		}
  		answer = Math.max(answer, sum1);
  		answer = Math.max(answer, sum2);
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[][] a = new int[n][n];
  		for(int i=0;i<n;i++) {
  			for(int j=0;j<n;j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n, a));
  	}
  }
  
  ```

## 10. 봉우리

- 내 풀이

  - 2x2배열이면 4x4배열로 다시 만듬

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  
  
  public class Ex10 {
  	public static int solution(int n,int[][] a) {
  		int answer = 0;
  		for(int i=1;i<=n;i++) {
  			for(int j=1;j<=n;j++) {
  				if((a[i][j] >a[i-1][j]) && (a[i][j] >a[i][j-1]) && (a[i][j] >a[i][j+1]) && (a[i][j] >a[i+1][j])) {
  					answer ++;
  				}
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[][] a = new int[n+2][n+2];
  		for(int i=1;i<=n;i++) {
  			for(int j=1;j<=n;j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n, a));
  	}
  }
  
  ```

- 풀이

  - 외각의 배열을 만드는게 아니라 있다고 생각하고 문제를 푼다
  - 경계선을 넘어가는 부분은 넘어가게 함

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  
  
  public class Sol10 {
  	public static int solution(int n,int[][] a) {
  		int[] dx = {-1,0,1,0};
  		int[] dy = {0,1,0,-1};
  		int answer = 0;
  		for(int i=0;i<n;i++) {
  			for(int j=0;j<n;j++) {
  				boolean flag =true;
  				for(int k=0;k<4;k++) {
  					int nx = i + dx[k]; //행좌표
  					int ny = j + dy[k];//열 좌표
  					//경계선도 설정
  					if(nx>=0 && nx<n && ny>=0&& ny<n && a[nx][ny] >= a[i][j]) { // 4방향의 값이 내가 중심이 값보다 크더라 => 봉우리가 아니다
  						flag= false;
  						break;
  					}
  				}
  				if(flag) {
  					answer ++;
  				}
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[][] a = new int[n][n];
  		for(int i=0;i<n;i++) {
  			for(int j=0;j<n;j++) {
  				a[i][j] = kb.nextInt();
  			}
  		}
  		System.out.println(solution(n, a));
  	}
  }
  
  ```

  

