## Solution 

Objective: Maximize profit by making any number of transactions (buy one and sell one share of the stock multiple times).

[Problem](https://www.codingninjas.com/studio/problems/selling-stock_630282?source=youtube&campaign=striver_dp_videos&leftPanelTabValue=PROBLEM)
Constraint: You can complete as many transactions as you like, but you must engage in another transaction only after selling the previous one.


Let's compare the two solutions, "buyandsell1" and "getMaximumProfit" (which is essentially "buyandsell2"), and list the topic tags and time complexity for each:

1. **buyandsell1:**
   ```java
   public int maxProfit(int[] prices) {
       int lsf = Integer.MAX_VALUE;
       int op = 0;
       int pist = 0;

       for (int i = 0; i < prices.length; i++) {
           if (prices[i] < lsf) {
               lsf = prices[i];
           }
           pist = prices[i] - lsf;
           if (op < pist) {
               op = pist;
           }
       }
       return op;
   }
   ```
   - **Topic Tags:**
     - Array
     - Dynamic Programming
   - **Time Complexity:**
     - O(n), where n is the length of the input array.

2. **getMaximumProfit (buyandsell2):**
   ```java
   public static long getMaximumProfit(int n, long[] values) {
       if (n <= 1) {
           return 0;
       }

       long maxProfit = 0;
       long mini = values[0];

       for (int i = 1; i < n; i++) {
           long curProfit = values[i] - values[i - 1];

           if (curProfit > 0) {
               maxProfit += curProfit;
           }
       }

       return maxProfit;
   }
   ```
   - **Topic Tags:**
     - Array
     - Greedy Algorithm
   - **Time Complexity:**
     - O(n), where n is the length of the input array.

**Comparison:**
- Both solutions are related to stock trading and involve finding the maximum profit.
- The first solution (`buyandsell1`) focuses on finding the maximum profit with a single transaction.
- The second solution (`getMaximumProfit` or `buyandsell2`) allows multiple transactions, maximizing profit by buying and selling stocks opportunistically.
- The topic tags for both include "Array," but the second solution also involves a "Greedy Algorithm" approach.
- The time complexity for both is O(n), making them efficient for large datasets.




1. **Base Case Check:**
   - If the length of the array (`n`) is less than or equal to 1, return 0 (no profit can be made).

2. **Initialize Variables:**
   - Initialize `maxProfit` to 0, representing the maximum profit.
   - Initialize `mini` to the first element of the array, representing the minimum stock value.

3. **Loop Over the Array:**
   - Calculate the current profit (`curProfit`) by subtracting the previous day's stock value from the current day's stock value.

4. **Check for Positive Profit:**
   - If `curProfit` is greater than 0 (indicating a positive profit), add it to `maxProfit`.

5. **Return Maximum Profit:**
   - After the loop, return the final `maxProfit`, which represents the maximum profit achievable through multiple transactions.

In summary, the code iterates through the array, calculates the daily profit, and accumulates positive profits to find the overall maximum profit that can be obtained through multiple stock transactions.
