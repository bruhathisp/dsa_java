## Solution

the solution is self explanatory


``` java
  class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n=prices.length;
        if(n<=1)
        {return 0;
        }

        int holdingProfit = -prices[0];  // Represents the maximum profit if holding a stock on the current day
        int sellingProfit = 0;          // Represents the maximum profit if not holding a stock on the current day

        for (int i = 1; i < prices.length; i++) {

            // Calculate the maximum profit if holding a stock on the current day
            holdingProfit = Math.max(sellingProfit - prices[i], holdingProfit);

            // Calculate the maximum profit if not holding a stock on the current day
            sellingProfit = Math.max(holdingProfit + prices[i] - fee, sellingProfit);
        }

        return sellingProfit;
    }
}

```
