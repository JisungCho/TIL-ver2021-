# 코딩테스트 - Stack, Queue(1)

## 1. 올바른 괄호

- 생각

  1. '(' 갯수와 ')'의 갯수를 각각 구해서 두 개의 갯수가 같으면 정답

     -> 문제 발생 : ))((의 경우 갯수는 같지만 옳은 경우가 아님

  2.  ( 가 등장하면 count를 늘려주고 만약 조회하는 동안에 count가 음수가 되면 이미 옳지 않으므로 NO가 되고 다 조회하고 count가 0이되면 YES ,count 양수가 되면  NO

  ```java
  package sec05;
  
  import java.util.Scanner;
  
  public class Ex01 {
  	public static String solution(String s) {
  		String answer = "NO";
  		int count = 0;
  		for(char c : s.toCharArray()) {
  			if(c == '(') {
  				count++;
  			}else {
  				count--;
  			}
  			if(count < 0) {
  				return answer;
  			}
  		}
  		if(count == 0) {
  			return answer= "YES";
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

- 풀이

  - stack 자료구조를 이용해서 ( 를 만나면 stack에다가 넣어주고 ) 를 만나면 stack에서 빼낸다

  ```java
  package sec05;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  import java.util.Stack;
  
  public class Sol01 {
  	public static String solution(String s) {
  		String answer = "YES";
  		
  		Stack<Character> stack = new Stack<>();
  		for(char c : s.toCharArray()) {
  			if(c == '(') {
  				stack.push(c);
  			}else {
  				if(stack.isEmpty()) return answer = "NO";
  				stack.pop();
  			}
  		}
  		if(!stack.isEmpty()) {
  			return answer ="NO";
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

## 2. 괄호문자제거

- 내 생각

  - 여는 괄호를 만나면 count초기화
  - 닫는 괄호를 만나면 pop 
  - 문자를 만나면 스택에 넣어줌

  => 문제점 :  count가 저장이 안됨 (A(bc)D)이면 bc가 빠지고 나서 ad가 빠져야하는데 d만 빠짐 count가 초기화되서

  ```java
  package sec05;
  
  import java.util.ArrayList;
  import java.util.Scanner;
  import java.util.Stack;
  
  public class Ex02 {
  	public static String solution(String s) {
  		String answer = "";
  		int count = 0;
  		Stack<Character> stack = new Stack<Character>();
  		for(char c : s.toCharArray()) {
  			if(c == '(') {
  				count = 0;
  			}else if(c == ')'){
  				for(int i=0;i<count;i++) {
  					stack.pop();
  				}
  			}else {
  				count++;
  				stack.push(c);
  			}
  		}
  		System.out.println(stack.toString());
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		String s = kb.nextLine();
  		System.out.println(solution(s));
  		
  	}
  }
  
  ```

- 풀이

  - 여는 괄호와 문자를 만나면 스택에다가 넣어줌
  - 닫는 괄호를 만나면 여는 괄호를 만날때까지 스택에서 빼줌

  ```java
  package sec05;
  
  import java.util.Scanner;
  import java.util.Stack;
  
  public class Ex02 {
  	public static String solution(String s) {
  		String answer = "";
  		Stack<Character> stack = new Stack<Character>();
  		for(char c : s.toCharArray()) {
  			if(c == ')') {
  				while(stack.pop() != '(');
  			}else {
  				stack.push(c);
  			}
  		}
  		for(int i=0;i<stack.size();i++) answer += stack.get(i);
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		String s = kb.nextLine();
  		System.out.println(solution(s));
  		
  	}
  }
  
  ```


## 3. 크레인 인형뽑기(카카오)

- 내생각 

  - peek 를 이용해서 스택 맨위를 조사해서 맨위와 같은 숫자가 있으면  pop하고 갯수를 2를 늘려줌

    ```java
    package sec05;
    
    import java.util.Scanner;
    import java.util.Stack;
    
    public class Ex03 {
    	public static int solution(int n,int[][] arr1 , int m, int[] arr2) {
    		int answer = 0;
    		Stack<Integer> stack = new Stack<>();
    		
    		for(int k : arr2) {
    			for(int i=0;i<n;i++) {
    				if(arr1[i][k-1] != 0) { //배열의 값이 0이 아니면
    					if(!stack.isEmpty()) { //스택이 비어있지 않은상태
    						if(stack.peek() == arr1[i][k-1]) { // 스택의 맨 위에 값과 내가 들어갈 값이 같은경우
    							stack.pop();
    							answer++;
    							answer++;
    						}else { //스택의 맨 위의 값과 내가 들어갈 값이 같지 않은경우
    							stack.push(arr1[i][k-1]);
    						}
    					}else { // 스택이 비어있는 상태
    						stack.push(arr1[i][k-1]);
    					}
    					arr1[i][k-1] = 0;
    					break;
    				}
    			}
    		}
    		return answer;
    	}
    	
    	public static void main(String[] args) {
    		Scanner kb = new Scanner(System.in);
    		
    		int n = kb.nextInt();
    		int[][] arr1 = new int[n][n];		
    		for(int i=0;i<n;i++) {
    			for(int k=0;k<n;k++) {
    				arr1[i][k] = kb.nextInt();
    			}
    		}
    		
    		int m = kb.nextInt();
    		int[] arr2 = new int[m];
    		for(int i=0;i<m;i++) {
    			arr2[i] = kb.nextInt();
    		}
    		
    		System.out.println(solution(n,arr1,m,arr2));
    	}
    }
    
    ```

- 풀이

  - peek를 이용

  ```java
  package sec05;
  
  import java.util.Scanner;
  import java.util.Stack;
  
  public class Sol03 {
  	public static int solution(int[][] arr1 , int[] arr2) {
  		int answer = 0;
  		Stack<Integer> stack = new Stack<>();
  		
  		for(int pos : arr2) {
  			for(int i=0; i<arr1.length;i++) { //크레인 내려감
  				if(arr1[i][pos-1] != 0) { //인형 발견
  					int tmp = arr1[i][pos-1];
  					arr1[i][pos-1] = 0;
  					if(!stack.isEmpty() && tmp == stack.peek()) {
  						answer += 2;
  						stack.pop();
  					}else { // 다른 인형이니까 push
  						stack.push(tmp);
  					}
  					break; //한번 처리하고 for문을 멈춤
  				}
  			}
  		}
  		
  		return answer;
  	}
  	
  	public static void main(String[] args) {
  		Scanner kb = new Scanner(System.in);
  		
  		int n = kb.nextInt();
  		int[][] arr1 = new int[n][n];		
  		for(int i=0;i<n;i++) {
  			for(int k=0;k<n;k++) {
  				arr1[i][k] = kb.nextInt();
  			}
  		}
  		
  		int m = kb.nextInt();
  		int[] arr2 = new int[m];
  		for(int i=0;i<m;i++) {
  			arr2[i] = kb.nextInt();
  		}
  		
  		System.out.println(solution(arr1,arr2));
  	}
  }
  
  ```

  