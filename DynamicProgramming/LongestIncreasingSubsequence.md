
---

### 🔍 Problem Statement:

> Given an integer array `nums`, return the **length of the longest strictly increasing subsequence**.

* A **subsequence** is a sequence you can get by deleting **some** elements (not necessarily contiguous).
* The numbers must be in **increasing** order.

---

### 🧠 Step-by-Step Internalization

#### ✅ Step 1: **What is the "state"?**

We’re trying to compute:

> `lis(i)` = **Length of the Longest Increasing Subsequence ending at index `i`**

Notice that:

* The sequence must **end** at `i`
* So we must **look back** and consider all previous elements `j < i`

---

#### 🪜 Step 2: **What are the "choices"?**

If we're at `i`, we can extend the LIS from a previous index `j` **if**:

```java
nums[j] < nums[i]
```

So:

```java
lis(i) = max(lis(j) + 1) for all j < i where nums[j] < nums[i]
```

---

#### 🔁 Step 3: **How to build the result from subproblems?**

Initialize all `lis[i] = 1`, because the smallest LIS at any index is the element itself.

Then loop:

```java
for i from 0 to n-1:
    for j from 0 to i-1:
        if nums[j] < nums[i]:
            lis[i] = max(lis[i], lis[j] + 1)
```

At the end, the **answer** is the max of all `lis[i]`.

---

### 🧠 Thought Process Without Code

#### Example: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`

We build the LIS **ending at each index**:

| Index | Num | LIS Value | Explanation                                           |
| ----- | --- | --------- | ----------------------------------------------------- |
| 0     | 10  | 1         | Nothing before it. So LIS ending here is just itself. |
| 1     | 9   | 1         | No previous smaller number.                           |
| 2     | 2   | 1         | No previous smaller number.                           |
| 3     | 5   | 2         | From 2 (at index 2), 2 → 5                            |
| 4     | 3   | 2         | From 2 → 3                                            |
| 5     | 7   | 3         | From 2 → 3 → 7 or 2 → 5 → 7                           |
| 6     | 101 | 4         | From 2 → 3 → 7 → 101 or other combinations            |
| 7     | 18  | 4         | From 2 → 3 → 7 → 18                                   |

Answer = `max(lis)` = **4**

---

### ✅ Java Code (Top-down DP)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] lis = new int[n];
        Arrays.fill(lis, 1); // Every element is a subsequence of length 1

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    lis[i] = Math.max(lis[i], lis[j] + 1);
                }
            }
        }

        int ans = 0;
        for (int x : lis) {
            ans = Math.max(ans, x);
        }

        return ans;
    }
}
```

---

### 🧠 How to Recognize This Pattern Again

Whenever a problem says:

> “Pick some elements (not necessarily adjacent), and make a sequence obeying a certain rule (e.g., increasing), what’s the max/min/...?”

You should:

1. Think about defining the subproblem as "ending at i"
2. Think of whether you can build from `j < i`
3. Check what the constraints are (increasing? non-decreasing?)
4. Update `dp[i]` based on all valid previous states

---

### 📌 Key Internalized Thought

> "At every position `i`, I’m trying to figure out what the best solution is **ending** at `i`. I build it by looking back at all `j < i`, and if `nums[j] < nums[i]`, I consider extending from there."

This pattern — **"for each i, look back at all j < i"** — is classic in DP problems involving **subsequences**.

---

