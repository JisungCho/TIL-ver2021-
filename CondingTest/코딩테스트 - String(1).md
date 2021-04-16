# CodingTest - 문자열 관련 문제1

## 1. 문자 찾기

- 문제

  한 개의 문자열을 입력받고, 특정 문자를 입력받아 해당 특정문자가 입력받은 문자열에 몇 개 존재하는지 알아내는 프로그램을 작성하세요.

- 내가 짠 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex01 {
  	public static void main(String[] args) {
  		Scanner scanner = new Scanner(System.in);
  		//문자열
  		String txt = scanner.next().toLowerCase();
  		//찾을 문자
  		char findText = scanner.next().toLowerCase().charAt(0);
  
  		int i =0;
  		int count = 0;
  		
  		while(i<txt.length()) {
  			if(findText == txt.charAt(i)) {
  				count ++;
  			}
  			i++;
  		}
  		System.out.println(count);
  	}
  }
  
  ```

- 정답 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Sol01 {
  	public static int solution(String str, char t) {
  		int answer = 0;
  		
  		//모두 대문자로 바꿈
  		str = str.toUpperCase();
  		t = Character.toUpperCase(t); 
  		
  		//갯수 새기
  		/*
  		for(int i =0;i<str.length();i++) {
  			if(str.charAt(i) == t ) {
  				answer++;
  			}
  		}
  		*/
  		//foreach에는 배열 , 컬렉션 프레임워크가 올수있다
  		//따라서 String 문자 배열로 만들어야함
  		for(char x : str.toCharArray()) {
  			if(x == t) {
  				answer++;
  			}
  		}
  		
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		//문자열 하나 읽음
  		String str = kb.next();
  		//문자 하나 읽음
  		//charAt : String을 인덱스로 접근
  		char c = kb.next().charAt(0);
  		
  		System.out.println(solution(str, c));
  	}
  }
  
  ```

## 2. 대소문자 변환

- 문제

  대문자와 소문자가 같이 존재하는 문자열을 입력받아 대문자는 소문자로 소문자는 대문자로 변환하여 출력하는 프로그램을 작성하세요.

- 내 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex02 {
  	public static String solution(String str) {
  		char[] c = str.toCharArray();
  		for(int i=0; i<c.length;i++) {
  			if(Character.isUpperCase(c[i])) {
  				c[i] = Character.toLowerCase(c[i]);
  			}else {
  				c[i] = Character.toUpperCase(c[i]);
  			}
  		}
  		return new String(c);
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String str = kb.next();
  		System.out.println(solution(str));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Sol2 {
  	public static String solution(String str) {
  		String answer = "";
  		for(char x : str.toCharArray()) {
  			if(Character.isLowerCase(x)) {
  				answer += Character.toUpperCase(x);
  			}else {
  				answer += Character.toLowerCase(x);
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

  - 아스키값 이용

    ```java
    package sec01;
    
    import java.util.Scanner;
    
    public class Sol2 {
    	public static String solution(String str) {
    		String answer = "";
    		for(char x : str.toCharArray()) {
    			if(x >= 97 && x<=122) { //소문자인경우
    				answer += (char)(x-32);
    			}else {
    				answer += (char)(x+32);
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

    

# 3. 문장 속 단어

- 문제

  한 개의 문장이 주어지면 그 문장 속에서 가장 긴 단어를 출력하는 프로그램을 작성하세요.

- 내 풀이

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex03 {
  	public static String solution(String str) {
  		String answer = "";
  		String[] strs = str.split(" ");
  		for(String x : strs) {
  			if(x.length() > answer.length()) {
  				answer = x;
  			}
  		}
  		return answer;
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String str = kb.nextLine();
  		System.out.println(solution(str));
  	}
  }
  
  ```

- 풀이

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Sol03 {
  	public static String solution(String str) {
  		String answer = "";
  		int max = Integer.MIN_VALUE; // 가장 작은 값으로 초기화
  		String[] s = str.split(" ");
  		for(String x : s) {
  			int len = x.length();
  			if(len > max) {
  				max = len;
  				answer = x;
  			}
  		}
  		return answer;
  	}
  
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		String str = kb.nextLine();
  		System.out.println(solution(str));
  	}
  }
  
  ```

  - indexOf , subString 이용

    ```java
    package sec01;
    
    import java.util.Scanner;
    
    public class Sol03 {
    	public static String solution(String str) {
    		String answer = "";
    		int max = Integer.MIN_VALUE , pos; // 가장 작은 값으로 초기화
    		while((pos = str.indexOf(' ')) != -1) { // 띄어쓰기가 발견하지 못하면 -1 , 발견하면 인덱스 번호 리턴
    			String tmp = str.substring(0,pos); // 0부터 pos전까지
    			int len = tmp.length();
    			if(len > max) {
    				max = len;
    				answer = tmp;
    			}
    			str = str.substring(pos+1); // 띄어쓰기 다음부터 다시 문자열 생성
    			//ex) it is time to ----> is time to
    		}
    		if(str.length() > max) { // 마지막 단어 체크
    				answer = str;
    		}
    		return answer;
    	}
    
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		String str = kb.nextLine();
    		System.out.println(solution(str));
    	}
    }
    
    ```

    

# 4. 단어 뒤집기

- 문제

- 개념

  - String , StringBuffer , StringBuilder

    - String vs StringBuffer/StringBuilder

      - String은 불변 

        ![](https://t1.daumcdn.net/cfile/tistory/99948B355E2F13350F)

      - StringBuffer / StringBuilder는 가변

        ![](https://t1.daumcdn.net/cfile/tistory/9923A9505E2F133608)

    - StringBuffer vs StringBuilder

      - StringBuffer : 멀티쓰레드 환경에서 안전
      - StringBuilder : 동기화를 지원하지 않지만 , 단일쓰레드에서의 성능은 StringBuffer보다 뛰어남 

- 내 코드

  ```java
  package sec01;
  
  import java.util.Scanner;
  
  public class Ex04 {
  	public static void solution(String[] str) {
  		for(String x: str) {
  			char[] array = x.toCharArray(); //good
  			for(int i=0;i<array.length-1;i++) { // 총 3번
  				for(int j=0;j<array.length-i-1;j++) { //  i=0 -> j=0 부터 j=4  , i=1 
  					char temp = array[j];
  					array[j] = array[j+1];
  					array[j+1] = temp;
  				}
  			}
  			x = String.valueOf(array);
  			System.out.println(x);
  		}
  	}
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		int n = kb.nextInt();
  		String[] str = new String[n];
  		for(int i=0;i<n;i++) {
  			str[i] = kb.next();
  		}
  		solution(str);
  	}
  }
  
  ```

  

- 풀이

  - StringBuilder 이용

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    public class Sol04 {
    	public static ArrayList<String> solution(int n, String[] str) {
    		//뒤짚은 단어들을 넣을 공간
    		ArrayList<String> answer = new ArrayList<>();
    		for(String x : str) {
    			/*
    			 * StringBuilder
    			 */
    			String tmp = new StringBuilder(x).reverse().toString();
    			answer.add(tmp);
    		}
    		return answer;
    	}
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		int n = kb.nextInt();
    		String[] str = new String[n];
    		for(int i=0;i<n;i++) {
    			str[i] = kb.next();
    		}
    		for(String x : solution(n, str)) {
    			System.out.println(x);
    		}
    	}
    }
    
    ```

    

  - 직접뒤집기

    ```java
    package sec01;
    
    import java.util.ArrayList;
    import java.util.Scanner;
    
    public class Sol04 {
    	public static ArrayList<String> solution(int n, String[] str) {
    		//뒤짚은 단어들을 넣을 공간
    		ArrayList<String> answer = new ArrayList<>();
    		for(String x : str) {
    			// lt 와 rt를 이용해서 직접 뒤지기!
    			char[] s = x.toCharArray();
    			int lt = 0;
    			int rt = x.length() - 1;
    			while(lt<rt) {
    				char tmp = s[lt];
    				s[lt] = s[rt];
    				s[rt] = tmp;
    				lt++;
    				lt--;
    			}
    			String tmp = String.valueOf(s);
    			answer.add(tmp);
    		}
    		return answer;
    	}
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		int n = kb.nextInt();
    		String[] str = new String[n];
    		for(int i=0;i<n;i++) {
    			str[i] = kb.next();
    		}
    		for(String x : solution(n, str)) {
    			System.out.println(x);
    		}
    	}
    }
    
    ```

  

