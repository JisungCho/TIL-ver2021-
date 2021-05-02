# 6. 최대길이 연속부분

- 내 생각

  - pr을 증가 0을 만나기전까지 0을 만나도 k가 있으면 pr을 증가
  - k가 없을 때 0을 만나면 lt를일단 lt의 값이 0이 아닐때 까지 증가시키고 0이나오면 pl을 증가

  ```java
  package sec03;
  
  import java.util.Scanner;
  
  public class Ex06 {
  	public static int solution(int n,int k ,int[] arr) {
  		int answer = 0 , pl = 0 ,max = Integer.MIN_VALUE;
  		for(int pr=0;pr<n;pr++) {			
  			if(k>0) { // k의 개수가 있으면
  				if(arr[pr] == 0) { // pr이 0인경우
  					k--;
  					answer++;
  				}else { // pr이 1인경우
  					answer++;
  				}
  			}else { //k의 개수가 없으면
  				if(arr[pr] == 0) { // pr의 값 0
  					while(arr[pl] != 0) {
  						pl++;
  						answer--;
  					}
  					pl++;
  				}else { //pr의 값 1
  					answer++;
  				}
  			}
  			if(max < answer) {
  				max = answer;
  			}
  		}
  		return max;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		int n = kb.nextInt(); //배열의 크기
  		int k = kb.nextInt(); // 0을 바꿀 수 있는 갯수
  		int[] arr = new int[n];
  		for(int i = 0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		
  		System.out.println(solution(n, k, arr));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec03;
  
  import java.util.Scanner;
  
  public class Sol06 {
  	public static int solution(int n,int k ,int[] arr) {
  		//cnt는 0을 1로 바꾼횟수
  		int answer = 0 , cnt =0 , lt =0;
  		for(int rt=0;rt<n;rt++) { //rt를 증가시킴
  			if(arr[rt] == 0 ) cnt++;  // rt의 값이 0이면 cnt를 증가시키면서 값 진행
  			while(cnt > k) { //지정된 횟수보다 커지면 lt를 조정 lt의 값이 0이면 cnt조정하고 1이면 lt만 증가
  				if(arr[lt] == 0) cnt--;
  				lt++;
  			}
  			answer = Math.max(answer, rt-lt+1);
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		int n = kb.nextInt(); //배열의 크기
  		int k = kb.nextInt(); // 0을 바꿀 수 있는 갯수
  		int[] arr = new int[n];
  		for(int i = 0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		
  		System.out.println(solution(n, k, arr));
  	}
  }
  
  ```

## 1. 학급 회장

- 풀이

  ```java
  package sec04;
  
  import java.util.HashMap;
  import java.util.Scanner;
  
  public class Ex01 {
  	public static char solution(int n,String s) {
  		char answer =' ';
  		HashMap<Character, Integer> map = new HashMap<>();
  		for(char key : s.toCharArray()) { // 알파벳 하나하나를 가져옴
   			map.put(key, map.getOrDefault(key, 0)+1);
  		}
  		//map에 특정키가 있는지 확인
  		//map.containsKey('A');
  		//key의 갯수
  		//map.size();
  		//특정 키 삭제
  		//map.remove('A');
  		
  		int max = Integer.MIN_VALUE;
  		for(char key : map.keySet()) {
  			if(map.get(key) > max) {
  				max = map.get(key);
  				answer = key;
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		int n = kb.nextInt(); //학생 수
  		String s = kb.next();
  		
  		System.out.println(solution(n, s));
  	}
  }
  
  ```



## 2. 아나그램

- 내 생각

  - HashMap에 담고나서 equals를 이용해서 비교

  ```java
  package sec04;
  
  import java.util.HashMap;
  import java.util.Scanner;
  
  public class Ex02 {
  	public static String solution(String s1 , String s2) {
  		HashMap<Character, Integer> map1 = new HashMap<>();
  		HashMap<Character, Integer> map2 = new HashMap<>();
  		for(char key : s1.toCharArray()) { // 각 글자수를 셈
  			map1.put(key, map1.getOrDefault(key, 0)+1);
  		}
  		for(char key : s2.toCharArray()) { // 각 글자수를 셈
  			map2.put(key, map2.getOrDefault(key, 0)+1);
  		}
  		
  		return map1.equals(map2) ? "YES" : "NO";
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		String s1 = kb.next();
  		String s2 = kb.next();
  		
  		System.out.println(solution(s1, s2));
  	}
  }
  
  ```

- 풀이

  - 일단 하나의 문자열을 HashMap에 키와 갯수를 넣어줌
  - 두번째 문자열을 하나씩 조회하면서 일단 해당 키가 있는지 조회 , 키가 있다면 HashMap의 해당 키값의 갯수를 감소시킴

  ```java
  package sec04;
  
  import java.util.HashMap;
  import java.util.Scanner;
  
  public class Sol02 {
  	public static String solution(String s1 , String s2) {
  		String answer = "YES";
  		HashMap<Character, Integer> map = new HashMap<>();
  		for(char x : s1.toCharArray()) {
  			map.put(x, map.getOrDefault(x, 0)+1);
  		}
  		for(char x : s2.toCharArray()) {
  			if(!map.containsKey(x) || map.get(x) == 0) {
  				return "NO";
  			}
  			map.put(x, map.get(x)-1);
  		}
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		String s1 = kb.next();
  		String s2 = kb.next();
  		
  		System.out.println(solution(s1, s2));
  	}
  }
  
  ```

  

