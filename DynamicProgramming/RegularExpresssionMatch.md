## Solution

[Watch this video ](https://youtu.be/HAA8mgxlov8?feature=shared)

![image](https://github.com/bruhathisp/dsa_java/assets/91585301/88eefa7f-6a0b-404e-b9d7-0f782761fae9)

[Watch this for bottoms-up this code](https://youtu.be/mNbzDlGKmLs?feature=shared)


Lessons learnt

1. In the context of wildcard pattern matching, the asterisk (*) symbol is used as a wildcard character that can match any sequence of characters (including an empty sequence). Here's what it represents:

- `*`: Matches any sequence of characters (including the empty sequence).

For example:

- Pattern: `"a*b"` can match strings like `"ab"`, `"aabb"`, `"aaaab"`, etc.
- Pattern: `"c*d*e"` can match strings like `"cde"`, `"ccddee"`, `"ccccdddeeeee"`, etc.

The asterisk (*) allows for flexible matching, where it can represent zero or more characters at that position in the pattern. It provides a way to match varying lengths of characters in the input string.



## Explanation

1. Consider the rows -string column- pattern
2.  Create a boolean nested array `boolean[][] dp `  it additionally stores the empty values like empty string and empty pattern `n+1`
and `m+1`



- If the current character in pattern `B` is '*', we look at the cell directly above (`dp[i - 1][j]`) or the cell directly to the left (`dp[i][j - 1]`). This is because '*' in the pattern can match either one or more characters, so we consider the possibility of either skipping a character in `A` (`dp[i - 1][j]`) or skipping a character in `B` (`dp[i][j - 1]`).

- If the current character in pattern `B` is '?' or matches the corresponding character in `A`, we look at the diagonal cell (`dp[i - 1][j - 1]`). This is because '?' in the pattern can match any single character, and when characters match, we move diagonally.

In simpler terms:

- If `B.charAt(j - 1) == '*'`, we check the upper (`dp[i - 1][j]`) and left (`dp[i][j - 1]`) cells.

- If `B.charAt(j - 1) == '?'` or `A.charAt(i - 1) == B.charAt(j - 1)`, we check the diagonal cell (`dp[i - 1][j - 1]`).

This way, we consider different possibilities based on the wildcard pattern rules, and the dynamic programming table `dp` is updated accordingly.

[Check this video at 40.35](https://youtu.be/DJvw8jCmxUU?feature=shared)

``` java
    public class Solution {
    public int isMatch(final String A, final String B) {
        int n = A.length(), m = B.length();  // rows -string column- pattern
        boolean[][] dp = new boolean[n + 1][m + 1]; // store the empty values like empty string and empty pattern

        // Base case: empty pattern matches with empty string
        dp[0][0] = true;

        // Handle patterns starting with '*'
        for (int i = 1; i <= m; i++) {
            if (B.charAt(i - 1) == '*')
                dp[0][i] = dp[0][i - 1];
        }

        // Dynamic programming to fill the dp table
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (B.charAt(j - 1) == '*') {
                    // '*' can match either one or more characters
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                } else if (B.charAt(j - 1) == '?' || A.charAt(i - 1) == B.charAt(j - 1)) {
                    // '?' matches any single character or characters match
                    dp[i][j] = dp[i - 1][j - 1];
                }
            }
        }

        // The final result is stored in the bottom-right cell of the dp table
        return dp[n][m] ? 1 : 0;
    }
}

```



#### Input
A: mississippi B. mis*i?*p*i
#### Output
1



Certainly! Here's the table in Markdown format with "mis*i?*p*i" as both row and column fields:


|     |   |   |   |   |   |   |   |   |   |   |   |
|-----|---|---|---|---|---|---|---|---|---|---|---|
|  Empty   |  Empty|    M | i | s | * | i | ? | * | p | * | i |
|-----|---|---|---|---|---|---|---|---|---|---|---|
| M   | T | F | F | F | F | F | F | F | F | F | F |
| i   | F | T | F | F | T | F | F | F | F | F | F |
| s   | F | F | T | F | F | F | F | F | F | F | F |
| *   | F | F | F | T | F | F | F | F | F | F | F |
| i   | F | F | F | F | T | F | F | F | F | F | F |
| ?   | F | F | F | F | T | T | F | F | F | F | F |
| *   | F | F | F | F | T | F | T | T | F | F | F |
| p   | F | F | F | F | T | F | F | T | F | F | F |
| *   | F | F | F | F | T | F | F | T | T | T | F |
| i   | F | F | F | F | T | F | F | T | F | T | T |



This table represents the dynamic programming matrix after the update (11, 10) for the given strings "Mississippi" and "mis*i?*p*i".


