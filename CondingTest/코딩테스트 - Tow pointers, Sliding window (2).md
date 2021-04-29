# 코딩테스트 - Tow pointers, Sliding window (2)

## 2. 공통원소구하기

- 문제

- 내 생각

  - 이중 for문을 돌면서 동일한 원소이면  arraylist에 넣어줌
  - 그러나 다 넣어주고나서 정렬하는것을 해결하지 못했음

- 풀이

  - 일단 두 개의 입력 배열을 오름차순으로 정렬하고나서 생각

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   * 두 개의 포인터를 사용해서 하나씩 비교
   */
  public class Ex02 {
  	public static ArrayList<Integer> solution(int n,int m,int[] a,int[] b){
  		ArrayList<Integer> answer = new ArrayList<>();
  		Arrays.sort(a);
  		Arrays.sort(b);
  		int p1=0, p2=0;
  		while(p1 < n && p2 < m) {
  			if(a[p1] == b[p2]) {
  				answer.add(a[p1]);
  				p1++;
  			}else if(a[p1]<b[p2]) {
  				p1++;
  			}else {
  				p2++;
  			}
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

## 3. 최대 매출

- 내 생각

  - pl을 처음 pr을 3일뒤로 잡아놓고 while문을 돌면서 sum에다가 저장하고 max랑 비교

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	최대매출
   */
  public class Ex03 {
  	public static int solution(int n,int k , int[] a){
  		int pl=0,pr=k-1; // 
  		int max = Integer.MIN_VALUE;
  		while(pr < n) {
  			int sum = 0;
  			for(int i=pl;i<=pr;i++) {
  				sum += a[i];
  			}
  			if(sum > max) {
  				max = sum;
  			}
  			pl++;
  			pr++;
  		}
  		
  		return max;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt();
  		int k = kb.nextInt();
  		int[] a = new int[n];
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, k, a));
  	}
  }
  
  ```

- 풀이

  - Sliding window를 이용해서  문제를 푼다 sum에다가 다음값을 누적시키고 처음값을 빼주면서 계속 증가시킨다.

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	최대매출
   */
  public class Sol03 {
  	public static int solution(int n,int k , int[] a){
  		int answer=0,sum=0;
  		for(int i=0;i<k;i++) {
  			sum += a[i];
  		}
  		answer = sum;
  		for(int i=k; i<n;i++) {
  			sum += (a[i]-a[i-k]);
  			answer = Math.max(answer, sum);
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt();
  		int k = kb.nextInt();
  		int[] a = new int[n];
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, k, a));
  	}
  }
  
  ```

## 4. 연속부분수열

- 내 생각

  - 1개씩비교 2개씩비교 3개씩비교 m 개씩비교
  - 맞을 수도 있었겠지만 시간 복잡도가 올라감

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	최대매출
   */
  public class Ex04 {
  	public static int solution(int n,int m , int[] a){
  		int answer=0;
  		int sum = 0;
  		int size = 1;
  		
  		while(size<=n) {
  			sum = 0;
  			for(int i=0;i<size;i++) {
  				sum += a[i];
  			}
  			if(sum == m) {
  				answer++;
  			}
  			for(int i=size;i<n;i++) {
  				sum +=  a[i]-a[i-size];
  				if(sum == m) {
  					answer++;
  				}
  			}
  			size++;
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt(); //갯수
  		int m = kb.nextInt(); // 목표 숫자
  		int[] a = new int[n]; 
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, m, a));
  	}
  }
  
  ```

- 풀이

  - 시간복잡도를 줄이기위해 lt와 rt를 이용 
  - 맨처음 sum에는 rt의 값을 대입

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	연속 부분수열
   */
  public class Sol04 {
  	public static int solution(int n,int m , int[] a){
  		int answer=0;
  		int sum = 0;
  		int lt=0;
  		for(int rt=0;rt<n;rt++) {
  			sum += a[rt]; 
  			if(sum == m) { //rt값을 더하면 바로확인
  				answer ++;
  			}
  			while(sum >= m) { // 목표값보다 sum이크면
  				sum -= a[lt++]; //lt값을 뺴고나서 lt증가
  				if(sum == m) { // lt값을 빼고난 sum값이 m과 같은지 확인
  					answer++;
  				}
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt(); //갯수
  		int m = kb.nextInt(); // 목표 숫자
  		int[] a = new int[n]; 
  		for(int i=0;i<n;i++) {
  			a[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, m, a));
  	}
  }
  
  ```

## 5. 연속된 자연수의 합

- 내 풀이

  - 모든 과정을 4번과 동일하게 품

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	연속 부분수열
   */
  public class Ex05 {
  	public static int solution(int n , int[] a){
  		int answer=0;
  		int sum = 0;
  		int lt=1;
  		for(int rt=1;rt<n;rt++) {
  			sum += a[rt]; 
  			if(sum == n) { //rt값을 더하면 바로확인
  				answer ++;
  			}
  			while(sum >= n) { // 목표값보다 sum이크거나 같으면
  				sum -= a[lt++]; //lt값을 뺴고나서 lt증가
  				if(sum == n) { // lt값을 빼고난 sum값이 m과 같은지 확인
  					answer++;
  				}
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt(); //목표 숫자
  		int[] a = new int[n]; 
  		for(int i=1;i<n;i++) {
  			a[i] = i;
  		}
  		System.out.println(solution(n,a));
  	}
  }
  
  ```

- 풀이

  - 연속된 자연수는 목표숫자의 /2 + 1 까지만 나온다.
  - 나머지 풀이과정은 4번 문제와 같음

  ```java
  package sec03;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.Scanner;
  
  /**
   * @author jisun
   *	연속 부분수열
   */
  public class Sol05 {
  	public static int solution(int n){
  		int answer=0 , sum = 0, lt = 0;
  		int m = n/2 + 1;  //몫 +1
  		int[] arr = new int[m];
  		for(int i=0;i<m;i++) {
  			arr[i] = i+1;
  		}
  		for(int rt=0;rt<m;rt++) {
  			sum += arr[rt];
  			if(sum == n) {
  				answer ++;
  			}
  			while(sum >= n) {
  				sum -= arr[lt++];
  				if(sum == n) {
  						answer++;
  				}
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt(); //목표 숫자
  		System.out.println(solution(n));
  	}
  }
  
  ```

  