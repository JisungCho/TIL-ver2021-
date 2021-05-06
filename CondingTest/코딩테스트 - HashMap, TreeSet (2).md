## 코딩테스트 - HashMap, TreeSet (2) 

## 3. 매출액의 종류

- 내 풀이  ( 해결 못함)

  - rt를 증가 
  - key - value를 숫자와 구역으로 설정

- 풀이

  - lt 와 rt를 둘다 둬서 증가시킴
  - key - value를 숫자와 갯수로 지정
  - lt가 증가하면 해당 lt로 되어있는 키의 값을 1감소시키고 해당 키의 값이 0이면 hashMap에서 지움

  ```java
  package sec04;
  
  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.Scanner;
  
  public class Ex03 {
  	public static ArrayList<Integer> solution(int n,int k ,int[] arr) {
  		HashMap<Integer, Integer> map = new HashMap<>();
  		int lt = 0 , rt = k-1; // 0 , 3
  		ArrayList<Integer> answer =  new ArrayList<>();
  		for(int i =0;i<=rt;i++) { //맨 처음 sling을 하기 위해 넣음
  			map.put(arr[i], map.getOrDefault(arr[i], 0)+1);
  		}
  		rt++;
  		answer.add(map.size());
  		for(int i=rt;i<n;i++) { 
  			map.put(arr[lt],map.get(arr[lt])-1); // lt의 값을 하나 없애줌			
  			map.put(arr[i], map.getOrDefault(arr[i], 0)+1);
  			if(map.get(arr[lt]) == 0) {
  				map.remove(arr[lt]);
  			}
  			lt++;
  			answer.add(map.size());
  		}
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		int n = kb.nextInt(); //일 수
  		int k = kb.nextInt(); //매출 구간
  		int[] arr = new int[n];
  		for(int i=0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		for(int i : solution(n, k, arr)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

## 4. 모든 아나그램 찾기

- 내 생각

  - 일단 두개의 문자열에 대해 HashMap을 각각 만들어서 문자-갯수 형태로 넣어줌
  - lt와 rt를 이용해서 비교하면서 equals메소드를 이용해 두 hashMap이 같은지 비교 같으면 아나그램 갯수 증가

  ```java
  package sec04;
  
  import java.util.HashMap;
  import java.util.Scanner;
  
  public class Ex04 {
  	public static int solution(String s, String t) {
  		int answer= 0 ;
  		int lt = 0, rt = t.length() - 1; //0,2
  		HashMap<Character, Integer> tHash = new HashMap<>();
  		HashMap<Character, Integer> sHash = new HashMap<>();
  		for(int i=0;i<t.length();i++) { //정답 애니그램
  			tHash.put(t.charAt(i), tHash.getOrDefault(t.charAt(i), 0)+1);
  		}
  		for(int i=0;i<t.length();i++) { //초기설정
  			sHash.put(s.charAt(i), sHash.getOrDefault(s.charAt(i), 0)+1);
  		}
  		
  		if(sHash.equals(tHash)) {
  			answer++;
  		}
  		rt++;
  		for(int i=rt;i<s.length();i++) {
  			sHash.put(s.charAt(lt), sHash.get(s.charAt(lt))-1);
  			sHash.put(s.charAt(i), sHash.getOrDefault(s.charAt(i), 0)+1);
  			if(sHash.get(s.charAt(lt)) == 0) {
  				sHash.remove(s.charAt(lt));
  			}
  			if(sHash.equals(tHash)) {
  				answer++;
  			}
  			lt++;
  		}
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		String s = kb.next();
  		String t = kb.next();
  		System.out.println(solution(s,t));
  	}
  }
  
  ```

- 풀이도 동일

## 5. K번째 큰 수

- 못품

  - 모든 경우의 수를 구하지 못함

- 풀이 

  - 모든 경우의 수를 구함 ( 3중 for문)
  - 합을 treeset에 넣어줌
  - treeset에서 3번째 큰 값을 가져옴

  ```java
  package sec04;
  
  import java.util.Collections;
  import java.util.Scanner;
  import java.util.TreeSet;
  
  public class Ex05 {
  	/*
  	 *  remove : 특정 값을 지워버림
  	 *  size : 크기
  	 *  first() : 오름차순이면 최소값 , 내림차순이면 최대값
  	 *  last() : 오름차순이면 최대값 , 내림차순이면 최소값
  	 */
  	public static int solution(int n,int k , int[] arr) {
  		int answer= -1 ;
  		TreeSet<Integer> Tset = new TreeSet<>(Collections.reverseOrder());
  		for(int i=0;i<n;i++) {
  			for(int j=i+1;j<n;j++) {
  				for(int l=j+1;l<n;l++) {
  					Tset.add(arr[i]+arr[j]+arr[l]);
  				}
  			}
  		}
  		int cnt = 0;
  		for(int x : Tset) {
  			cnt ++;
  			if(cnt == k) {
  				return x;
  			}
  		}
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		 
  		int n = kb.nextInt();
  		int k = kb.nextInt();
  		int[] arr = new int[n];
  		for(int i=0;i<n;i++) {
  			arr[i] = kb.nextInt();
  		}
  		System.out.println(solution(n,k,arr));
  	}
  }
  
  ```

  