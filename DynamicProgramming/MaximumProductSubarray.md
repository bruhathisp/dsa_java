Absolutely! Let’s break down **Leetcode 152. Maximum Product Subarray** in a **super simple, beginner-friendly way**, just like we did for the matrix path problem.

---

## 🧠 Problem Summary

You're given an array of integers (can include **negatives**, **positives**, and **zeroes**).
You need to find a **contiguous subarray** (continuous slice of the array) such that the **product of its elements is the maximum possible**.

---


## 🤯 Why is this problem tricky?

Because:

* A **negative number** can turn a **big positive into a negative**.
* But two negatives can also create a **positive**.
* A **zero** resets everything.

So, we can’t just keep track of **only the maximum** so far — we must track:

> Both the **maximum** and **minimum** product up to this point.

---

## 🤔 Core Idea (Very Important)

At every index `i`, we track:

* `maxProd[i]`: the **maximum product** ending at index `i`.
* `minProd[i]`: the **minimum product** ending at index `i`.

Why the min too?

Because:
➡️ A small **negative product** might become **big positive** later
➡️ Example: `[-2, -3]` → product = `6`

---

## 🪜 Step-by-Step Plan

We move **left to right**, and for each number:

1. We remember the **current number**
2. We multiply it with:

   * The previous **max product**
   * The previous **min product**
3. We choose the best option.

We also update a global result: the **biggest product seen so far**

---


Take this as an example [2, 3, 0, -2, 4, 5, -7 ] 


tempmax         2 6 0 0   4  20 -7
tempmin         2 2 0 -2 -6 -30 210



``` java
public class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;

        int maxSoFar = nums[0];
        int currMax = nums[0];
        int currMin = nums[0];

        for (int i = 1; i < n; i++) {
            int tempMax = currMax;
            int tempMin = currMin;

            currMax = Math.max(nums[i], Math.max(tempMax * nums[i], tempMin * nums[i]));
            currMin = Math.min(nums[i], Math.min(tempMax * nums[i], tempMin * nums[i]));

            maxSoFar = Math.max(maxSoFar, currMax);
        }

        return maxSoFar;
    }
}


```
