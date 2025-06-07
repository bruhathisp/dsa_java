 **Let's derive the full solution** for **Maximum Subarray Sum** step by step, **from first principles**.

---

## 🧠 Problem Recap:

> Given an integer array `nums`, find the **maximum sum of a contiguous subarray**.

We’ll walk through the 5 core DP thinking questions — **and answer them with code, example, and intuition.**

---

## ✅ 1️⃣ Can I define a subproblem?

Yes!

> Let’s define `dp[i]` as the **maximum sum of a subarray ending at index `i`**.

This means:

* We’re not solving the whole array at once.
* We’re solving "what's the best subarray *ending* here?"

---

## ✅ 2️⃣ Do I want to start fresh or continue?

Great question!

At every index `i`, we ask:

```java
dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);
```

**Why?**

* If adding `nums[i]` to the previous sum helps → continue.
* If the previous sum is dragging us down → start fresh.

🔁 This “reset or continue” is the **core** of Kadane’s Algorithm.

---

## ✅ 3️⃣ What should I track at every step?

Track two things:

1. `dp[i]` – best sum ending at index `i`
2. `maxSum` – the best sum **overall** (global max)

```java
int[] dp = new int[n];
dp[0] = nums[0];
int maxSum = dp[0];
```

---

## ✅ 4️⃣ How do I get the final result?

Just take the maximum over all `dp[i]`:

```java
maxSum = Math.max(maxSum, dp[i]);
```

---

## ✅ 5️⃣ What is my base case?

Start at index `0`.
The best subarray **ending at 0** is just the number itself.

```java
dp[0] = nums[0];
```

---

## 💡 Full Java Code with Comments

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];

        // Base case: best sum ending at 0 is just nums[0]
        dp[0] = nums[0];
        int maxSum = dp[0];

        for (int i = 1; i < n; i++) {
            // Either start fresh or extend previous subarray
            dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);

            // Track the global maximum
            maxSum = Math.max(maxSum, dp[i]);
        }

        return maxSum;
    }
}
```

---

## 🔍 Walkthrough with Example: `[-2,1,-3,4,-1,2,1,-5,4]`

| i | nums\[i] | dp\[i]              | maxSum |
| - | -------- | ------------------- | ------ |
| 0 | -2       | -2                  | -2     |
| 1 | 1        | max(1, -2 + 1) = 1  | 1      |
| 2 | -3       | max(-3, 1 - 3) = -2 | 1      |
| 3 | 4        | max(4, -2 + 4) = 4  | 4      |
| 4 | -1       | max(-1, 4 - 1) = 3  | 4      |
| 5 | 2        | max(2, 3 + 2) = 5   | 5      |
| 6 | 1        | max(1, 5 + 1) = 6   | 6 ✅    |
| 7 | -5       | max(-5, 6 - 5) = 1  | 6      |
| 8 | 4        | max(4, 1 + 4) = 5   | 6      |

---

## 🎯 Final Takeaway

> At each index, ask: “Is it better to start over, or continue?”
> Then keep track of the best *ending here* and the best *overall*.

That’s **Kadane’s Algorithm**, framed through **pure DP thinking**.

---
