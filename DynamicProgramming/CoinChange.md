## Solution

Let’s now apply the **5-question method** to the **Coin Change** problem 

---

## 🪙 **Coin Change Problem**

**Problem Statement:**
Given coins of different denominations and a total amount, return the *minimum number of coins* needed to make up that amount. If that amount cannot be made, return -1.

### Example:

```text
coins = [1, 2, 5], amount = 11
Answer: 3 (5 + 5 + 1)
```

---

## ✅ The 5 Questions (and Answers for Coin Change)

---

### **1. Can I break this problem into smaller subproblems?**

> ❓ *Can solving for smaller amounts help solve the bigger amount?*

**Yes.**
To get amount `11`, I can:

* Try using a `1` coin → then solve for `10`
* Try using a `2` coin → then solve for `9`
* Try using a `5` coin → then solve for `6`

So, if I can find the **minimum coins for smaller amounts**, I can use them to find the answer for the bigger amount.

---

### **2. What is my “state”? What do I need to remember?**

> ❓ *What info do I need to build the solution step-by-step?*

Let’s define:

```java
dp[x] = minimum coins needed to make amount x
```

So, the **state** is: for every `x`, what's the minimum number of coins needed.

---

### **3. What is my “transition”? How do I go from one state to another?**

> ❓ *How does dp\[x] depend on previous values?*

Try every coin:

```java
dp[x] = min(dp[x - coin] + 1 for all coin in coins)
```

Why `+1`? Because you're using one coin now, and adding it to the best solution of a smaller amount.

---

### **4. What is the base case (starting point)?**

> ❓ *What do I already know without doing anything?*

**dp\[0] = 0**
No coins are needed to make amount 0.

---

### **5. What is the final answer I care about?**

> ❓ *Which dp value is my final goal?*

**dp\[amount]**
If it's still infinity (unreachable), return `-1`. Otherwise, return `dp[amount]`.

---

## 🧠 Summary Table for Coin Change

| Question                   | Answer                                 |
| -------------------------- | -------------------------------------- |
| 1. Break into subproblems? | Yes, subtract a coin and solve smaller |
| 2. State                   | `dp[x] = min coins to make amount x`   |
| 3. Transition              | `dp[x] = min(dp[x - coin] + 1)`        |
| 4. Base Case               | `dp[0] = 0`                            |
| 5. Final Answer            | `dp[amount]`, or `-1` if unreachable   |

---

## ✨ Code (Java)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1); // fill with "infinity"
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (i >= coin) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

### 🧩 How to Internalize

* Imagine building a wall of height `amount`.
* At each level, you ask: "Can I reach here by placing a 1, 2, or 5-height block below it?"
* You solve all smaller heights before solving a taller one (bottom-up).
* DP remembers the best way to get to smaller amounts so you don’t redo work.

---

Would you like to visualize this step-by-step (JavaScript tracer style) or move on to the next problem like **Longest Palindromic Substring** or **Knapsack** using the same method?
