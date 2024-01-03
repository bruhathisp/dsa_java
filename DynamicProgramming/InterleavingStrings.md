## Solution

[WATCH THIS](https://youtu.be/3Rw3p9LrgvE?feature=shared)

Lessons Leant:

1. An interleaved string is formed by merging two strings in a way that maintains the relative order of characters from both strings.
2. 

Steps:

1. `dp[i][j] =0` if `i=j=0`  top-left cell element base case.
2. Fill the first row, `i=0` where string `A` is empty. It checks AND
   1. If the current character in string `B` matches the corresponding character in string `C` (`dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1)`).
   2. And if the left element is true.
3. Fill the first column, `j=0` where string `B` is empty. It checks AND
   1. if the current character in string `A` matches the corresponding character in string `C` (`dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1)`).
   2. and if the above element is true

- The final `else` block handles the general case of interleaving. It checks two conditions OR
    - The diagonal cell (`dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1)`),
    - The left cell (`dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1)`)  // THE LAST C IN THE STRING B(J-1)==C(I+J-1)

In summary, the code fills the DP table by considering various cases based on the positions of `i` and `j`, and it checks the diagonal, left, up, and down cells to determine whether the interleaving is possible.
  


``` java
    public class Solution {
    public static int isInterleave(String A, String B, String C) {
        if (C.length() != A.length() + B.length()) {
            return 0;
        }

        boolean dp[][] = new boolean[A.length() + 1][B.length() + 1];

        for (int i = 0; i <= A.length(); i++) {
            for (int j = 0; j <= B.length(); j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = true;
                } else if (i == 0) {
                    dp[i][j] = dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1);
                } else if (j == 0) {
                    dp[i][j] = dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1);
                } else {
                    dp[i][j] = (dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1)) ||
                               (dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1));
                }
            }
        }

        return dp[A.length()][B.length()] ? 1 : 0;
    }

    public static void main(String args[]) {
        System.out.println(isInterleave("B", "e", "Be"));
    }
}
```


### Testcase


Input: aabcc  dbbca   aadbbcbcac

Output: 1


   |  Empty | d |b |b |c |a |
---|---|---|---|---|---|---|
 Empty | T | F | F | F | F | F |
---|---|---|---|---|---|---|
 a | T | F | F | F | F | F |
---|---|---|---|---|---|---|
 a | T | T | T | T | T | F |
---|---|---|---|---|---|---|
 b | F | T | T | F | T | F |
---|---|---|---|---|---|---|
 c | F | F | T | T | T | T |
---|---|---|---|---|---|---|
 c |  F | F | F | T | F | T|
---|---|---|---|---|---|---|

Result: 1



## Code to try the above (Try in custom input in interview bits)


``` java
    public class Solution {
    public static int isInterleave(String A, String B, String C) {
        if (C.length() != A.length() + B.length()) {
            return 0;
        }

        boolean dp[][] = new boolean[A.length() + 1][B.length() + 1];

        for (int i = 0; i <= A.length(); i++) {
            for (int j = 0; j <= B.length(); j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = true;
                } else if (i == 0) {
                    dp[i][j] = dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1);
                } else if (j == 0) {
                    dp[i][j] = dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1);
                } else {
                    dp[i][j] = (dp[i - 1][j] && A.charAt(i - 1) == C.charAt(i + j - 1)) ||
                               (dp[i][j - 1] && B.charAt(j - 1) == C.charAt(i + j - 1));
                }
            }
        }
        
        System.out.println("Final values:");
        for (int i = 0; i <= A.length(); i++) {
            for (int j = 0; j <= B.length(); j++) {
                System.out.print(dp[i][j] + " ");
            }
            System.out.println();
        }



        return dp[A.length()][B.length()] ? 1 : 0;
    }

    public static void main(String args[]) {
        System.out.println(isInterleave("B", "e", "Be"));
    }
}


```
