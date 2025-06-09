

Imagine you are trying to cut a long bar (the `nums` array) into `m` parts. Your goal is:

> Make the cuts **in such a way** that the *largest piece* is **as small as possible**.

Now here’s what `curSum` and `maxSum` are doing:

---

### ✅ `curSum`

* This is the **sum of the current piece** you are considering.
* You are trying different places to make the first cut.
* As you go from left to right, `curSum` keeps adding the values.

🧠 Think of it like:

> "Okay, if I make my first cut **here**, the first piece would sum up to `curSum`."

---

### ✅ `dfs(j + 1, m - 1)`

* This tells you:

> "If I cut here, and now split the **rest of the array** into `m-1` parts, what is the **minimum largest piece I can get** in that future?"

---

### ✅ `maxSum = Math.max(curSum, dfs(j + 1, m - 1))`

* You're comparing the **current piece's sum** and the **largest piece from the future parts**.
* Why? Because you care about the **largest piece overall** after the split.

> This is the worst piece size *if* you cut here.

---

### ✅ `res = Math.min(res, maxSum)`

* Out of all possible cut positions, you want the **cut that makes the worst piece as small as possible**.
* So keep the **minimum of all max values** (that’s `res`).

---

### 🪚 A simple metaphor:

* Say you’re cutting cake into 3 plates.
* You try:

  * Give first person a small piece → rest is harder to balance.
  * Give first person a bigger piece → others may get smaller slices.
* Try all options.
* Pick the **cut** that gives **everyone the most balanced pieces**.

---


## Using momoization  Top Down

``` java
import java.util.*;

public class Solution {
    public int splitArray(int[] nums, int k) {
        int n = nums.length;
        int[] prefixSum = new int[n + 1];

        // Step 1: Compute prefix sum for easy range sum lookup
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }

        // Memoization table: dp[i][k] = min largest sum for splitting nums[i..n-1] into k parts
        Map<String, Integer> dp = new HashMap<>();

        // Step 2: Call recursive function
        return dfs(0, k, nums, prefixSum, dp);
    }

    private int dfs(int i, int k, int[] nums, int[] prefixSum, Map<String, Integer> dp) {
        int n = nums.length;

        // Base case: one group left → take all remaining
        if (k == 1) {
            return prefixSum[n] - prefixSum[i];
        }

        // Check memo
        String key = i + "," + k;
        if (dp.containsKey(key)) return dp.get(key);

        int res = Integer.MAX_VALUE;

        // Try different cut positions
        for (int j = i; j <= n - k; j++) {
            int curSum = prefixSum[j + 1] - prefixSum[i]; // sum from i to j
            int future = dfs(j + 1, k - 1, nums, prefixSum, dp); // solve rest
            int maxSplit = Math.max(curSum, future);
            res = Math.min(res, maxSplit);

            // Optional break to speed up
            if (curSum > res) break;
        }

        dp.put(key, res);
        return res;
    }
}


```




## Using interation (Bottom up better)

``` java

public class Solution {
    public int splitArray(int[] nums, int k) {
        int n = nums.length;
        int[][] dp = new int[n + 1][k + 1];
        int[] prefixSum = new int[n + 1];

        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }

        for (int i = 0; i <= n; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }

        dp[0][0] = 0; // 0 elements into 0 parts = 0 sum

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= k; j++) {
                for (int p = 0; p < i; p++) {
                    int current = prefixSum[i] - prefixSum[p];
                    dp[i][j] = Math.min(dp[i][j], Math.max(dp[p][j - 1], current));
                }
            }
        }

        return dp[n][k];
    }
}


```
