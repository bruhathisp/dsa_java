

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


Main Intuition:

---

So, you split at some point `p`, and the current sum of the last part is `prefixSum[i] - prefixSum[p]`. This gives you the sum of the subarray from `p` to `i-1`. Now, to find the **worst-case (largest) part sum** for this split, you compare this current sum with the largest sum from the earlier parts, which is stored in `dp[p][j-1]`. You take the `max(dp[p][j-1], current)` because you care about the biggest piece in that split, and then across all possible `p`, you want to find the split that gives the **minimum** of these maximums.

For example, say you want to split the first 4 numbers into 3 parts (`i = 4`, `j = 3`), and your array is `[26, 3, 5, 11]`. You try splitting at `p = 1`. That divides the array into:

* First part: `nums[0] = 26`
* Last part: `nums[1..3] = [3, 5, 11]`

The sum of the last part is `prefixSum[4] - prefixSum[1] = 45 - 26 = 19`.
Then you look at `dp[1][2]`, which is the best way to split the first element (`nums[0]`) into 2 parts — but since we can't split one number into 2 parts, `dp[1][2] = INF`, so we skip it.

But if you try another valid `p`, say `p = 2`, then:

* First 2 numbers `[26, 3]` are split into 2 parts (from `dp[2][2]`)
* Last part is `[5, 11]`, sum = `prefixSum[4] - prefixSum[2] = 45 - 29 = 16`
* And `dp[2][2] = 26`, so worst-case part sum = `max(26, 16) = 26`

You try all such `p` and pick the split where that `max()` is smallest — that becomes your `dp[4][3]`.

---




nums = [26, 3, 5, 11, 15, 25]

prefixSum = [0, 26, 29, 34, 45, 60, 85]



---

### 🔁 We're looping over all `p < i`

We are calculating `dp[i][j]` — the **minimum largest sum** you can get by splitting `nums[0..i-1]` into `j` parts.

Each `p` is a **split point** — we imagine the last part starts at index `p`.

---

### 🧠 So, what does that mean?

* You split the array at index `p`.
* You assume:

  * The first `j-1` parts are `nums[0..p-1]` → so the best you can do for this is `dp[p][j-1]`.
  * The **j-th part** (last one) is `nums[p..i-1]`, and its sum is `current = prefixSum[i] - prefixSum[p]`.

---

### ✨ Intuition:

You're asking:

> “If I make the last subarray from `p` to `i-1`, and I already split everything before `p` into `j-1` parts in the best possible way (dp\[p]\[j-1])... what’s the worst sum (biggest subarray sum) in this configuration?”

The answer is:

```java
max(dp[p][j-1], current)
```

Because one part could be very large — and we always care about the **worst-case (largest)** part size.

---

### 🎯 Goal:

Find the best place to split (`p`) such that the **maximum subarray sum is minimized**.

```java
dp[i][j] = min over all valid p of: max(dp[p][j-1], prefixSum[i] - prefixSum[p])
```

---

### ✅ TL;DR:

* You’re splitting at `p`
* The **last subarray** is `nums[p..i-1]` (length `i - p`)
* The **previous subarrays** are handled by `dp[p][j - 1]`
* You compare the largest part between those two (`max`)
* Then, over all possible `p`, take the **min** of these maximums

---



nums = [26, 3, 5, 11, 15, 25]

prefixSum = [0, 26, 29, 34, 45, 60, 85]

So prefixSum[3] = 34

Entire DP table

``` css
DP Table (dp[i][j]):
i\j   0     1     2     3
-----------------------------
0   |  0   INF   INF   INF
1   | INF  26    INF   INF
2   | INF  29     26   INF
3   | INF  34     26    26
4   | INF  45     26    26
5   | INF  60     31    26
6   | INF  85     45    31

```


for each `dp[3][j]` value:

---

### ✅ `dp[3][1]`

```plaintext
dp[3][1] = min(INF, max(dp[0][0], 34)) = min(INF, max(0, 34)) = 34
```

(Other `p = 1, 2` are skipped because `dp[1][0]`, `dp[2][0]` = INF)

---

### ✅ `dp[3][2]`

```plaintext
dp[3][2] = min(INF, max(dp[1][1], 8)) = min(INF, max(26, 8)) = 26
dp[3][2] = min(26, max(dp[2][1], 5)) = min(26, max(29, 5)) = 26
```

---

### ✅ `dp[3][3]`

```plaintext
dp[3][3] = min(INF, max(dp[2][2], 5)) = min(INF, max(26, 5)) = 26
```

(Other `p = 0, 1` skipped since `dp[0][2]`, `dp[1][2]` = INF)

---



