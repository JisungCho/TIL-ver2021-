# 코딩테스트 - 문자열2

## 특정 문자 뒤집기

- 문제

  영어 알파벳과 특수문자로 구성된 문자열이 주어지면 영어 알파벳만 뒤집고,특수문자는 자기 자리에 그대로 있는 문자열을 만들어 출력하는 프로그램을 작성하세요.

- 내 생각

  lt 와 rt를 이용해서 특수문자인 경우에는 비교하지 않고 lt와 rt의 값만 변화시켜주고 둘다 특수문자가 아닐경우에만 바꿔줌

- 내 코드

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Ex05 {
  	public static String solution(String str) {
  		char[] c = str.toCharArray();
  		int lt = 0; //맨처음
  		int rt = c.length -1 ; // 맨끝
  		while(lt < rt) {
  			if(((32<= c[lt]) && (c[lt] <=47)) || ((58<= c[lt]) && (c[lt] <=64)) || ((91<= c[lt]) && (c[lt] <=96)) || ((123<= c[lt]) && (c[lt] <=126)))  {//특수 문자
  				lt++;
  			}else if(((32<= c[rt]) && (c[rt] <=47)) || ((58<= c[rt]) && (c[rt] <=64)) || ((91<= c[rt]) && (c[rt] <=96)) || ((123<= c[rt]) && (c[rt] <=126)))  {//특수 문자
  				rt--;
  			}else {
  				char tmp = c[lt];
  				c[lt] = c[rt];
  				c[rt] = tmp;
  				lt++;
  				rt--;
  			}
  		}
  		String answer = String.valueOf(c);
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Sol05 {
  	public static String solution(String str) {
  		char[] c = str.toCharArray();
  		int lt = 0; //맨처음
  		int rt = c.length -1 ; // 맨끝
  		while(lt < rt) {
  			if(!Character.isAlphabetic(c[lt]))  {//특수 문자
  				lt++;
  			}else if(!Character.isAlphabetic(c[rt]))  {//특수 문자
  				rt--;
  			}else {
  				char tmp = c[lt];
  				c[lt] = c[rt];
  				c[rt] = tmp;
  				lt++;
  				rt--;
  			}
  		}
  		String answer = String.valueOf(c);
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	}
  }
  
  ```

## 중복문자제거

- 문제

  소문자로 된 한개의 문자열이 입력되면 중복된 문자를 제거하고 출력하는 프로그램을 작성하세요.

  중복이 제거된 문자열의 각 문자는 원래 문자열의 순서를 유지합니다.

- 내 생각

  - 처음부터 하나씩 비교해서 같은 문자가 나오면 0으로 바꾼다
  - 0이 아닐 경우만 answer에 넣어줌

- 내 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Ex06 {
  	public static String solution(String str) {
  		char[] c = str.toCharArray();
  		String answer = "";
  		for (int i = 0; i < c.length; i++) { // 처음부터 마지막 인덱스 전까지
  			for (int j = i+1; j < c.length; j++) {
  				if (c[i] == c[j]) { // 두개의 값비교
  					c[j] = 0;
  				}
  			}
  			if(c[i] != 0) {
  				answer += c[i];
  			}
  		}
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Sol06 {
  	public static String solution(String str) {
  		String answer = "";
  		for(int i=0;i<str.length();i++) {
  			//문자의 위치 인덱스와  문자가 처음 등장하는 인덱스를 보여줌
  			//따라서 문자의 위치 인덱스와 문자가 처음 등장하는 인덱스가 같으면 중복되지 않은 문자
  			//System.out.println(str.charAt(i)+" "+i+ " "+str.indexOf(str.charAt(i)));
  			
              //str.charAt(i) : 현재 인덱스의 문자
              //str.indexOf() : 문자가 등장하는 최초인덱스
  			//현재인덱스의 문자가 등장하는 최초인덱스가 현재 인덱스랑 같으면 중복되지 않은 문자
  			if(str.indexOf(str.charAt(i))== i) {
  				answer += str.charAt(i);
  			}
  		}
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	
  }
  
  ```

  

## 회문 문자열

- 문제

  앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 회문 문자열이라고 합니다.

  문자열이 입력되면 해당 문자열이 회문 문자열이면 "YES", 회문 문자열이 아니면 “NO"를 출력하는 프로그램을 작성하세요.

  단 회문을 검사할 때 대소문자를 구분하지 않습니다

- 생각

  - lt랑 rt랑 비교해서 값이 값으면 회문

- 내 코드

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Ex07 {
  	public static String solution(String str) {
  		//모든 글자를 소문자로 변환
  		char[] c = str.toLowerCase().toCharArray();
  		int lt = 0 , rt = c.length-1;
  		while(lt<rt) {
  			if(c[lt] != c[rt]) {
  				return "NO";
  			}
  			lt++;
  			rt--;
  		}
  		return "YES";
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	}
  }
  
  ```

  

- 풀이

  - 1번 풀이

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    public class Sol07 {
    	public static String solution(String str) {
    		String answer= "YES";
    		str = str.toUpperCase();
    		int len = str.length();
    		for(int i=0;i<len/2;i++) {
    			if(str.charAt(i) != str.charAt(len-i-1)) {
    				return answer = "NO";
    			}
    		}
    		
    		return answer;
    	}
    
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String s = kb.next();
    		System.out.println(solution(s));
    	}
    }
    
    ```

  - 2번 풀이 (StringBuilder 이용)

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    public class Sol07 {
    	public static String solution(String str) {
    		String answer= "NO";
    		//뒤짚은 글자
    		String tmp = new StringBuilder(str).reverse().toString();
    		//뒤짚은 글자가 같으면 equals는 대소문자 구분
    		//equalsIgnoreCase로 비교
    		if(str.equalsIgnoreCase(tmp)) {
    			answer = "YES";
    		}
    		return answer;
    	}
    
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String s = kb.next();
    		System.out.println(solution(s));
    	}
    }
    
    ```

    

## 유효한 팰린드롬 (replaceAll 정규식이용)

- 문제

- 생각

- 내 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Ex08 {
  	public static String solution(String str) {
  		
  		//소문자로 변환후 소문자 a-z까지 빼고 전부 공백으로 바꿈
  		String s1 = str.toLowerCase().replaceAll("[^a-z]", "");
  		String s2 = new StringBuilder(s1).reverse().toString();
  		if(s1.equals(s2)) {
  			return "YES";
  		}
  		return "NO";
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.nextLine();
  		System.out.println(solution(s));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Sol08 {
  	public static String solution(String str) {
  		String answer= "NO";
  		str = str.toUpperCase().replaceAll("[^A-Z]","");
  		String tmp = new StringBuilder(str).reverse().toString();
  		if(str.equals(tmp)) {
  			return answer = "YES";
  		}
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.nextLine();
  		System.out.println(solution(s));
  	}
  }
  
  ```

  

## 숫자만 추출

- 문제

- 생각

  - parseInt(): 원시데이터인 int 타입을 반환
    valueOf(): Integer 래퍼(wrapper)객체를 반환

- 내 코드

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  
  public class Ex09 {
  	public static int solution(String str) {
  		int answer = 0;
  		str = str.replaceAll("[^0-9]", "");
  		//parseInt를 사용하면 앞에 0이 지워짐
  		int i = Integer.parseInt(str);
  		return answer = i;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		System.out.println(solution(s));
  	}
  }
  
  ```

- 풀이

  - 아스키이용

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    /*
     *  
     */
    public class Sol09 {
    	public static int solution(String str) {
    		int answer = 0;
    
    		for(char x : str.toCharArray()) {
    			if(x>=48 && x<=57) {
    				answer = answer * 10 +(x-48);
    				//0 = 0 x10 + 0
    				// 1 = 0x10 +1 
    				// 12 =    1 x10 +2
    				//120  = 12x10 + 0
    				//1205 = 120x10 + 5
    				
    			}
    		}
    		
    		return answer;
    	}
    
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String s = kb.next();
    		System.out.println(solution(s));
    	}
    }
    
    ```

  - String 이용

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    public class Sol09 {
    	public static int solution(String str) {
    		String answer = "";
    
    		for(char x : str.toCharArray()) {
    				if(Character.isDigit(x)) {
    					answer += x;
    			}
    		}
    		//0208이 숫자가 되면 앞에 0이사라짐
    		return Integer.parseInt(answer);
    	}
    
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String s = kb.next();
    		System.out.println(solution(s));
    	}
    }
    
    ```

## 가장 짧은 문자거리

- 문제

- 힌트

  - 방법1

    teachermode에서 e를 기준으로 거리를 구한다면 먼저 각각의 e (여기서는 e가 세번 나옴)를 기준으로 길이를 구한다 

    1 0 1 2 3 4 5 6 7 8 9 , 5 4 3 2 1 0 1 2 3 4 5  , 10 9 8 7 6 5 4 3 2 1 0 앞에서 부터 한자리씩 비교하면서 가장 작은 숫자를 선택

    ==> 실패

  - 방법2

    p라는 기준거리를 만들어놓고 for문을 돌면서 원하는 문자를 만나면 거리를 0으로 만들고 p를 +1을 해주고 다시 원하는 문자를 만나면 0으로 만듬 그리고 for문을 왼쪽에서 오른쪽으로 오른쪽에서 왼쪽으로 2번 돌림

- 내 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.List;
  import java.util.Scanner;
  
  public class Ex10 {
  	public static int[] solution(String str,char c) {
  		// 정담을 담을 배열
  		int[] answer =new int[str.toCharArray().length];
  		//문자가 들어가있는 character배열
  		char[] tmp = str.toCharArray();
  		//임시거리
  		int p = 1000;
  		
  		
  		for(int i=0;i<tmp.length;i++) {
  			if(tmp[i] == c) {
  				p = 0 ;
  				answer[i] = p;
  			}else {
  				answer[i] = p;
  			}
  			p++;
  		}
  		
  		p=1000;
  		
  		for(int i=tmp.length-1;i>=0;i--) {
  			if(tmp[i] == c) {
  				p = 0 ;
  				answer[i] = p;
  			}else {
  				if(answer[i] >p) {
  					answer[i] = p;
  				}
  			}
  			p++;
  		}
  		
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		char c = kb.next().charAt(0);
  		for(int i : solution(s, c)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.ArrayList;
  import java.util.List;
  import java.util.Scanner;
  
  public class Sol10 {
  	public static int[] solution(String str,char c) {
  		// 정담을 담을 배열
  		int[] answer =new int[str.length()];
  		int p = 1000;
  		
  		//왼쪽에서 오른쪽
  		for(int i=0;i<str.length();i++) {
  			if(str.charAt(i) == c ) {
  				p=0;
  				answer[i] = p;
  			}else {
  				p++;
  				answer[i] = p;
  			}
  		}
  		
  		p = 1000;
  		
  		//오른쪽에서 왼쪽
  		for(int i=str.length()-1;i>=0;i--) {
  			if(str.charAt(i) == c ) {
  				p=0;
  			}else {
  				p++;
  				//둘중에서 작은 값으로
  				answer[i] = Math.min(answer[i], p);
  			}
  		}
  		
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String s = kb.next();
  		char c = kb.next().charAt(0);
  		for(int i : solution(s, c)) {
  			System.out.print(i+" ");
  		}
  	}
  }
  
  ```

  