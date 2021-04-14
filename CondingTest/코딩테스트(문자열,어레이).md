# 코딩테스트

## 11. 문자열 압축

- 문제

  알파벳 대문자로 이루어진 문자열을 입력받아 같은 문자가 연속으로 반복되는 경우 반복되는

  문자 바로 오른쪽에 반복 횟수를 표기하는 방법으로 문자열을 압축하는 프로그램을 작성하시오.

  단 반복횟수가 1인 경우 생략합니다.

- 생각

  1. 앞에서부터 하나씩 조회
  2. 다음 문자를 조회해서 동일한 문자이면 카운트 증가
  3. 동일한 문자가 아니면 해당문자와 카운트 출력

- 내 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex11 {
  	public static String solution(String str) {
  		String answer="";
  		int count = 1;
  		for(int i=0;i<str.length()-1;i++) { //같은경우
  			if(str.charAt(i) == str.charAt(i+1)) {
  				count ++;
  				if(i == str.length()-2) { //마지막인경우
  					answer += str.charAt(i);
  					answer += String.valueOf(count);
  				}
  			}else { //다른경우
  				if(i == str.length()-2) {
  					answer += str.charAt(i);
  					answer += String.valueOf(count);
  					answer += str.charAt(i+1);
  				}else {
  					answer += str.charAt(i);
  					if(count > 1) {
  						answer += String.valueOf(count);
  					}
  					count = 1;	
  				}
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String str = kb.next();
  		System.out.println(solution(str));
  	}
  }
  
  ```

- 풀이

  - 들어온 입력 문자열 끝에 공백 문자를 출력하여 마지막에 i 와 i+1이 비교 할 수 있게끔 한다.

  - 공백을 추가하지 않으면 Outofbounce 에러 발생

    ```java
    package sec01;
    
    import java.util.Scanner;
    
    public class Sol11 {
    	public static String solution(String str) {
    		String answer="";
    		str += " ";
    		int count = 1;
    		for(int i=0;i<str.length()-1;i++) { //같은경우
    			if(str.charAt(i) == str.charAt(i+1)) {
    				count ++;
    			}else { //다른경우
    					answer += str.charAt(i);
    					if(count > 1) {
    						answer += String.valueOf(count);
    					}
    					count = 1;	
    				}
    		}
    		return answer;
    	}
    	
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String str = kb.next();
    		System.out.println(solution(str));
    	}
    }
    
    ```

## 12. 암호

- 문제

  \#****###**#####**#####**##**

  이 신호를 4개의 문자신호로 구분하면

  \#****## --> 'C'

  \#**#### --> 'O'

  \#**#### --> 'O'

  \#**##** --> 'L'

  최종적으로 “COOL"로 해석됩니다.

- 생각

  - substring
  - pareInt
  - 형변환

- 내 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex12 {
  	public static String solution(int count,String str) {
  		String answer="";
  		for(int i=0;i<count;i++) {
  			String tmp = str.substring(0,7);
  			tmp = tmp.replace('#', '1');
  			tmp = tmp.replace('*','0');
  			int demical = Integer.parseInt(tmp, 2);
  			answer += (char)demical;
  			str = str.substring(7);
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int count = kb.nextInt();
  		String str = kb.next();
  		System.out.println(solution(count,str));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Sol12 {
  	public static String solution(int n,String s) {
  		String answer="";
  		for(int i=0;i<n;i++) {
  			String tmp = s.substring(0, 7).replace('#', '1').replace('*', '0');
  			int num = Integer.parseInt(tmp, 2);
  			answer += (char)num;
  			s = s.substring(7);
  		}
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		String str = kb.next();
  		System.out.println(solution(n,str));
  	}
  }
  
  ```


# 섹션2 Array

## 1. 큰 수 출력하기

- 문제

  N개의 정수를 입력받아, 자신의 바로 앞 수보다 큰 수만 출력하는 프로그램을 작성하세요.

  (첫 번째 수는 무조건 출력한다)

- 생각

  - ArrayList사용

- 풀이 == 내 코드

  ```java
  package sec02;
  
  import java.util.ArrayList;
  import java.util.List;
  import java.util.Scanner;
  
  public class Ex01 {
  	public static  List<Integer> solution(int n,int[] arr) {
  		List<Integer> answer = new ArrayList<>();
  		answer.add(arr[0]);
  		for(int i=1;i<arr.length;i++) {
  			if(arr[i] > arr[i-1]) {
  				answer.add(arr[i]);
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i=0;i<arr.length;i++) {
  			arr[i] = kb.nextInt();
  		}
  		for(int x : solution(n, arr)) {
  			System.out.print(x+" ");
  		}
  	}
  }
  
  ```

## 2. 보이는 학생

- 문제

  선생님이 N명의 학생을 일렬로 세웠습니다. 일렬로 서 있는 학생의 키가 앞에서부터 순서대로 주어질 때, 맨 앞에 서 있는

  선생님이 볼 수 있는 학생의 수를 구하는 프로그램을 작성하세요. (앞에 서 있는 사람들보다 크면 보이고, 작거나 같으면 보이지 않습니다.)

- 개념

  - 처음은 무조건 카운트
  - max라는 변수를 만듬 - 자기 앞에 학생들 중에서 가장 키큰 사람의 키
  - max가 바뀌면 카운트 함
  - 2중 for문을 쓰면 데이터의 갯수가 많아지면 오류가 발생함

- 내 코드

  - 2중 for문

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Ex02 {
  	public static  int solution(int n,int[] arr) {
  		int answer=0;
  		LOOP1 : 
  		for(int i=arr.length-1;i>=0;i--) {
  			LOOP2 :
  			for(int j=0;j<i;j++) {
  				if(arr[j] >= arr[i]) {
  					continue LOOP1;
  				}
  			}
  		answer ++;
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i=0;i<arr.length;i++) {
  			arr[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, arr));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Sol02 {
  	public static  int solution(int n,int[] arr) {
  		int answer = 1;
  		int max = arr[0];
  		
  		for(int i=1;i<n;i++) {
  			if(arr[i] > max) {
  				answer++;
  				max = arr[i];
  			}
  		}
  	
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i=0;i<arr.length;i++) {
  			arr[i] = kb.nextInt();
  		}
  		System.out.println(solution(n, arr));
  	}
  }
  
  ```

## 3. 가위 바위 보

- 문제
- 개념
  - a와 b중에 하나에 기준을 맞추고 분석

- 내 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Ex03 {
  	public static char[] solution(int n,int[] a,int[] b) {
  		char[] c = new char[n];
  		for(int i=0;i<n;i++) {
  			if(a[i] == 1) {
  				if(b[i]== 1 ) { //비김
  					c[i] = 'D';
  				}else if(b[i] == 2) { // 짐
  					c[i] = 'B';
  				}else { // 이김
  					c[i] ='A' ;
  				}
  			}else if(a[i] == 2) {
  				if(b[i]== 1 ) { //이김
  					c[i] = 'A';
  				}else if(b[i] == 2) { // 비김
  					c[i] = 'D';
  				}else { //짐
  					c[i] = 'B';
  				}
  			}else {
  				if(b[i]== 1 ) { //짐
  					c[i] = 'B';
  				}else if(b[i] == 2) { // 이김
  					c[i] = 'A';
  				}else { // 비김
  					c[i] = 'D';
  				}
  			}
  		}
  		return c;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] a = new int[n];
  		int[] b = new int[n];
  		for(int i=0;i<a.length;i++) {
  			a[i] = kb.nextInt();
  		}
  		for(int i=0;i<b.length;i++) {
  			b[i] = kb.nextInt();
  		}
  		for(char c : solution(n, a, b)) {
  			System.out.println(c);
  		}
  	}
  }
  
  ```

- 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Sol03 {
  	public static String solution(int n,int[] a,int[] b) {
  		String answer="";
  		for(int i=0;i<n;i++) {
  			if(a[i] == b[i]) answer +="d"; // 비기는경우
  			else if(a[i] == 1 && b[i] == 3) answer += "A"; // A가 1로 이기는경우
  			else if(a[i] == 2&& b[i] == 1) answer += "A"; // A가 2로 이기는 경우
  			else if(a[i] == 3 && b[i] == 2) answer += "A"; // A가 3으로 이기는경우
  			else { // 그 외는 B가 이기는경우
  				answer += "B";
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		int[] a = new int[n];
  		int[] b = new int[n];
  		for(int i=0;i<a.length;i++) {
  			a[i] = kb.nextInt();
  		}
  		for(int i=0;i<b.length;i++) {
  			b[i] = kb.nextInt();
  		}
  		for(char x : solution(n, a, b).toCharArray()) {
  			System.out.println(x);
  		}
  	}
  }
  
  ```

## 4. 피보나치 수열

- 문제

- 개념

  - 앞에 1 1 은 무조건 고정

- 내 풀이

  ```java
  package sec02;
  
  import java.util.Scanner;
  
  public class Ex04 {
  	public static int[] solution(int n) {
  			int[] a = new int[n];
  			a[0] = 1;
  			a[1] = 1;
  			for(int i=0;i<n-2;i++) {
  				a[i+2] = a[i] + a[i+1];
  			}
  			return a;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		for(int i : solution(n)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

- 풀이

  - 배열사용

    ```java
    package sec02;
    
    import java.util.Scanner;
    
    public class Sol04 {
    	public static int[] solution(int n) {
    			int[] a = new int[n];
    			a[0] = 1;
    			a[1] = 1;
    			for(int i=2;i<n;i++) {
    				a[i] = a[i-2] + a[i-1];
    			}
    			return a;
    	}
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		int n = kb.nextInt();
    		for(int i : solution(n)) {
    			System.out.print(i+" ");
    		}
    	}
    }
    
    ```

  - 배열미사용

    ```java
    package sec02;
    
    import java.util.Scanner;
    
    public class Sol04 {
    	public static void solution(int n) {
    		int a = 1;
    		int b = 1;
    		int c;
    		System.out.print(a+" "+b+" ");
    		for(int i=2;i<n;i++) {
    			c = a+b;
    			System.out.print(c+" ");
    			a =  b;
    			b =  c;
    		}
    	}
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		int n = kb.nextInt();
    		solution(n);
    	}
    }
    
    ```

    

  