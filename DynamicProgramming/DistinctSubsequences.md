## Solution

1. Initialise dp[i][0] as 1. (first column)
2. if `A.charAt(i - 1) == B.charAt(j - 1)` then `dp[i][j]` is the sum of diagonal left element and the above element
3. else `dp[i][j]` is the above element.
4. return the element `dp[m][n]`

   [Watch 10.50](https://youtu.be/nVG7eTiD2bY?feature=shared)

### Code 

``` java
     public class Solution {
    public int numDistinct(String A, String B) {
        
        final int m = A.length();
    final int n = B.length();
    long[][] dp = new long[m + 1][n + 1];

    for (int i = 0; i <= m; ++i)
      dp[i][0] = 1;

    for (int i = 1; i <= m; ++i)
      for (int j = 1; j <= n; ++j)
        if (A.charAt(i - 1) == B.charAt(j - 1))
          dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
        else
          dp[i][j] = dp[i - 1][j];

    return (int) dp[m][n];
    }
}

```

#### Testcase

Input 2:
    A = "rabbbit" 
    B = "rabbit"

Output 2:
    3


### Output


|       | Empty | r | a | b | b | i | t |
|-------|---|---|---|---|---|---|---|
| **Empty** | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| **r** | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| **a** | 1 | 1 | 1 | 0 | 0 | 0 | 0 |
| **b** | 1 | 1 | 1 | 1 | 0 | 0 | 0 |
| **b** | 1 | 1 | 1 | 2 | 1 | 0 | 0 |
| **b** | 1 | 1 | 1 | 3 | 3 | 0 | 0 |
| **i** | 1 | 1 | 1 | 3 | 3 | 3 | 0 |
| **t** | 1 | 1 | 1 | 3 | 3 | 3 | 3 |



### Code to print the above 


``` java
  public class Main {
    public static int numDistinct(String A, String B) {
        final int m = A.length();
        final int n = B.length();
        long[][] dp = new long[m + 1][n + 1];

        // Print initial values
        System.out.println("Initial values:");
        for (int i = 0; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                System.out.print(dp[i][j] + " ");
            }
            System.out.println();
        }

        // Initialize the base case: any string has one subsequence of an empty string
        for (int i = 0; i <= m; ++i)
            dp[i][0] = 1;

        // Dynamic programming to fill the dp array
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (A.charAt(i - 1) == B.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j];

                // Print information for each iteration
                System.out.println("i=" + i + ", j=" + j + ", A[i-1]=" + A.charAt(i - 1) +
                        ", B[j-1]=" + B.charAt(j - 1) + ", dp[" + i + "][" + j + "]=" + dp[i][j]);
            }
        }

        // Print the final values
        System.out.println("Final values:");
        for (int i = 0; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                System.out.print(dp[i][j] + " ");
            }
            System.out.println();
        }

        // Return the final result, which is the number of distinct subsequences
        return (int) dp[m][n];
    }
    
    public static void main(String args[]){
        String A = new String("rabbbit");
        String B = new String("rabbit");
        System.out.println(numDistinct(A,B));
    }
}


```
