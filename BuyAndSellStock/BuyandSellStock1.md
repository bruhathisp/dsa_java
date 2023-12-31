## Solution



The given Java solution aims to find the maximum profit that can be achieved by buying and selling a stock `at most one time` represented by an array of `prices`. The variable names used in the solution have the following meanings:


Concept Tag:
- Dynamic Programming
- Stock Trading
- One Pass Algorithm
  A one-pass algorithm is an algorithm that processes the input data only once, without the need to revisit or reprocess the data. 
  A classic example of a one-pass algorithm is finding the maximum or minimum element in an array. Instead of comparing elements in multiple passes, a one-pass algorithm would update the maximum or minimum element as it processes each element of the array.
- Array Traversal


The given Java solution uses a simple one-pass algorithm to find the maximum profit that can be achieved by buying and selling a stock represented by the `prices` array. The logic behind the solution is as follows:

1. Initialize three variables:
   - `lsf` (lowest so far): Set it to `Integer.MAX_VALUE`, representing the lowest stock price encountered so far during the iteration.
   - `op` (overall profit): Set it to 0, representing the maximum profit achieved so far.
   - `pist` (profit if sold today): Set it to 0 initially.

2. Iterate through the `prices` array using a loop. For each element at index `i`:

   a. Check if the current stock price (`prices[i]`) is less than the current `lsf`. If it is, update `lsf` to the current stock price. This step ensures that `lsf` always represents the lowest price encountered so far.

   b. Calculate the profit that can be obtained if the stock is sold at the current price (`pist = prices[i] - lsf`). This step calculates the profit that can be made if the stock is bought on `lsf` and sold at the current price.

   c. Update `op` to the maximum of the current `op` and `pist`. This step ensures that `op` stores the maximum profit obtained so far.

3. After the loop, the variable `op` will contain the maximum profit that can be achieved by buying and selling the stock.

Here's a step-by-step explanation of the given example:

Example 1: Input: [7, 1, 5, 3, 6, 4]
1. Initialize `lsf` to `Integer.MAX_VALUE`, `op` to 0, and `pist` to 0.
2. Loop through the `prices` array:
   - prices[0] = 7, lsf = 7 (lowest value seen so far), pist = 0 (not possible to sell yet).
   - prices[1] = 1, lsf = 1 (lowest value seen so far), pist = 1 - 1 = 0 (not possible to sell yet).
   - prices[2] = 5, lsf = 1 (no change in lsf), pist = 5 - 1 = 4 (possible profit if sold at 5).
   - prices[3] = 3, lsf = 1 (no change in lsf), pist = 3 - 1 = 2 (possible profit if sold at 3).
   - prices[4] = 6, lsf = 1 (no change in lsf), pist = 6 - 1 = 5 (possible profit if sold at 6).
   - prices[5] = 4, lsf = 1 (no change in lsf), pist = 4 - 1 = 3 (possible profit if sold at 4).
3. The maximum value in `pist` is 5, which means the maximum profit is 5.



```java
class Solution {
    public int maxProfit(int[] prices) {
        int lsf = Integer.MAX_VALUE;
        int op = 0;
        int pist = 0;
        
        for(int i = 0; i < prices.length; i++){
            if(prices[i] < lsf){
                lsf = prices[i];
            }
            pist = prices[i] - lsf;
            if(op < pist){
                op = pist;
            }
        }
        return op;
    }
}
```
