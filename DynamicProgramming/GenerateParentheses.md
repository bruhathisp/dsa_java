## Solution

Lessons Learnt:

1. `generateParentheseshelper(current + "(", open + 1, close, n, result);` observe closely.  The return to the helper function should be `String, int,int,int,String`
2. O(4^n / n ) no idea how this is the time complexity. I'll be updating this ai generated answer. hint Catalan number

``` java

  import java.util.ArrayList;
import java.util.List;


class Solution {
	List<String> generateParentheses(int n) {
	    // add your logic here
		
		List<String>  result= new ArrayList<>();
		generateParentheseshelper("",0,0,n,result);
		
		return result;
		
	}
	public static void generateParentheseshelper(String current, int open, int close, int n, List<String> result){
		
		if (current.length()==2*n){

// Base case: if the length of the current combination is equal to 2*n, add it to the result
			
			result.add(current);
			
			
			return ; 
			
		}
		
		
		if(open <n){
    // Recursive case: add an open parenthesis if the count of open parentheses is less than 'n'

			generateParentheseshelper(current+"(",open+1, close,n,result);
			
		}
		
		if(close<open){
			// Recursive case: add a close parenthesis if the count of close parentheses is less than the count of open parentheses
			generateParentheseshelper(current+")",open, close+1,n,result);
			
			
		}
		
		
	}
	
}
```
