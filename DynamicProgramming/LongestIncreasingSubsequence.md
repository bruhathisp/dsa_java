
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

# O(n log n) Solution 



---

## ✅ Problem Statement (LIS)

You're given an integer array `nums`. Return the **length** of the **longest increasing subsequence (LIS)**.
A **subsequence** is a selection of elements that keeps **relative order** (but not necessarily contiguous).
Each element in the subsequence must be **strictly increasing**.

Example:
Input: `[0, 1, 0, 3, 2, 3]`
Output: `4` (LIS: `[0, 1, 2, 3]`)

---

## 💡 Core Internalized Idea

We want to **track the smallest possible "tail"** of all increasing subsequences of different lengths.

We use a list `tails` where:

* `tails[i]` = the **smallest tail** of all increasing subsequences of length `i + 1` seen so far.
* Eg: [0 1 3] vs [0 1 2] we are going to choose [0 1 2]

Why the **smallest**?

> Because a smaller tail gives us **more room** to build longer increasing subsequences later.
> 💬 "A subsequence of length 3 ending in 2 gives more room than ending in 3."

---

### 🔍 Step-by-Step Example: `[0, 1, 0, 3, 2, 3]`

We'll maintain `tails[]` as we iterate:

#### `num = 0`

* `tails = []` → empty → append `0`
  ✅ `tails = [0]`

#### `num = 1`

* `1 > 0` → extend increasing subsequence → append
  ✅ `tails = [0, 1]`

#### `num = 0`

* `0 <= 0` → can't extend, but can **replace** with something smaller
  ✅ `tails = [0, 1]` (0 replaces 0 — no effect here)

#### `num = 3`

* `3 > 1` → extend LIS → append
  ✅ `tails = [0, 1, 3]`

#### `num = 2`

* `2 < 3` → can't extend LIS, but can **replace 3 with 2**
  ✅ `tails = [0, 1, 2]`

**🧠 Why replace 3 with 2?**
Because we’re saying:

> “Now I know a way to build a subsequence of length 3 ending in 2 — that’s better than ending in 3 because it leaves more space to grow.”
> → This is **greedy** but safe because we only track **lengths**, not actual sequences.

#### `num = 3`

* `3 > 2` → extend
  ✅ `tails = [0, 1, 2, 3]`

---

## 🔁 Final Insight

The **length** of `tails[]` at the end gives the **length of LIS**.
It’s **not the actual LIS**, but it's the minimum ending values you’ve seen for subsequences of each possible length.

---

## ✅ Java Code

```java
import java.util.*;

class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> tails = new ArrayList<>();

        for (int num : nums) {
            int left = 0, right = tails.size();
            while (left < right) {
                int mid = (left + right) / 2;
                if (tails.get(mid) < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }

            if (left == tails.size()) {
                tails.add(num); // Extend LIS
            } else {
                tails.set(left, num); // Replace to allow better growth
            }
        }

        return tails.size(); // Length of LIS
    }
}
```

---

## ✨ TL;DR

* Track **smallest possible tail** for all increasing subsequences of each length.
* **Replace** if possible → leads to better future options.
* `tails.length` = answer.

---


