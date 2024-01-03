## Solution

The concatenating two strings is based on the overlap between the last characters of the first string and the first characters of the second string.


- **Rows (1st Dimension):**
  - Each row in `dp` corresponds to a specific combination of strings.
  - The number of rows is determined by `2^n`, where `n` is the number of strings in the input. `[1 << n]`  means shifting the binary representation of 1 to the left by n positions. In other words, it calculates 2 to the power of n. 
  - For example, if `n = 3`, there are 2^3 = 8 possible combinations of strings (from 000 to 111 in binary).

- **Columns (2nd Dimension):**
  - Each column corresponds to an individual string in the input.
  - The number of columns is equal to the number of strings `n`.

- **Values:**  initialize the dp array with a very large value 
  - The value in `dp[i][j]` represents the minimum cost of combining the strings in the subset represented by the binary number `i` (in the 1st dimension) and ending with the string at index `j` (in the 2nd dimension).


### Get Cost 

`getCost`, is responsible for determining the cost of appending one string (`b`) after another string (`a`). The cost is calculated based on the overlap between the end of string `a` and the beginning of string `b`. Let's break down the code:


1. **Initialization:**
   - `cost` is initialized with the length of string `b`. This is because, initially, the cost of appending `b` is considered to be its full length.

2. **Find Minimum Length:**
   - `minLength` is `min(a.length(), b.length())`.  This ensures that we only check overlapping parts up to the length of the shorter string.

3. **Overlap Check:**
   - iterate from 1 to `minLength`. check if the last `k` characters of string `a` (`a.substring(a.length() - k)`) match the first `k` characters of string `b` (`b.substring(0, k)`).
   - For example, if a = "abcd", then a.substring(a.length() - 1) would result in the substring "d", which is the last character of the string.

4. **Update Cost:**
   - If a match is found, it updates the `cost` to (`b.length() - k`).

5. **Return Cost:**




``` java
 import java.util.ArrayList;
import java.util.Arrays;

public class Solution {
    public int solve(ArrayList<String> A) {
        final int n = A.size();
        // cost[i][j] := the cost to append A[j] after A[i]
        int[][] cost = new int[n][n];
        // dp[s][j] := the minimum cost to visit nodes of s ending in j, s is a
        // binary Value, e.g. dp[6][2] means the minimum cost to visit {1, 2} ending
        // in 2 (6 = 2^1 + 2^2)
        int[][] dp = new int[1 << n][n];
        for (int i = 0; i < dp.length; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE / 2);
        }
        // parent[s][j] := the parent of "nodes of s ending in j"
        int[][] parent = new int[1 << n][n];
        for (int i = 0; i < parent.length; i++) {
            Arrays.fill(parent[i], -1);
        }

        for (int i = 0; i < n; ++i)
            for (int j = i + 1; j < n; ++j) {
                cost[i][j] = getCost(A.get(i), A.get(j));
                cost[j][i] = getCost(A.get(j), A.get(i));
            }

        for (int i = 0; i < n; ++i)
            dp[1 << i][i] = A.get(i).length();

        for (int s = 1; s < (1 << n); ++s)
            for (int i = 0; i < n; ++i) {
                if ((s & (1 << i)) == 0)
                    continue;
                for (int j = 0; j < n; ++j)
                    if (dp[s - (1 << i)][j] + cost[j][i] < dp[s][i]) {
                        dp[s][i] = dp[s - (1 << i)][j] + cost[j][i];
                        parent[s][i] = j;
                    }
            }

        String ans = "";
        int j = getLastNode(dp[(1 << n) - 1]);
        int s = (1 << n) - 1; // 2^0 + 2^1 + ... + 2^(n - 1)

        // Traverse back to build the string.
        while (s > 0) {
            final int i = parent[s][j];
            if (i == -1)
                ans = A.get(j) + ans;
            else
                ans = A.get(j).substring(A.get(j).length() - cost[i][j]) + ans;
            s -= 1 << j;
            j = i;
        }

        return ans.length();
    }

    // Returns the cost to append b after a.
    private int getCost(final String a, final String b) {
        int cost = b.length();
        final int minLength = Math.min(a.length(), b.length());
        for (int k = 1; k <= minLength; ++k)
            if (a.substring(a.length() - k).equals(b.substring(0, k)))
                cost = b.length() - k;
        return cost;
    }

    private int getLastNode(int[] row) {
        int minIndex = 0;
        for (int i = 1; i < row.length; ++i)
            if (row[i] < row[minIndex])
                minIndex = i;
        return minIndex;
    }
}


```
