## Solution





``` java
    public class StockProfitCalculator {
    public int calculateMaxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }

        int maxProfitAfterBuying = -prices[0];
        int maxProfitAfterSelling = 0;
        int maxProfitBeforeSellingCooldown = 0;

        for (int currentDay = 1; currentDay < prices.length; currentDay++) {
            // Calculate the maximum profit after buying on the current day or continuing from the previous day's buying
            maxProfitAfterBuying = Math.max(maxProfitBeforeSellingCooldown - prices[currentDay], maxProfitAfterBuying);

            // Keep track of the maximum profit one day before selling (cooldown)
            int temp = maxProfitAfterSelling;

            // Calculate the maximum profit after selling on the current day or continuing from the previous day's selling
            maxProfitAfterSelling = Math.max(maxProfitAfterBuying + prices[currentDay], maxProfitAfterSelling);

            // Update the maximum profit one day before selling (cooldown)
            maxProfitBeforeSellingCooldown = temp;
        }

        // Return the final maximum profit achievable
        return maxProfitAfterSelling;
    }
}


```





| **Day** | **Initial Max Profit After Buying** `max(maxProfitBeforeSellingCooldown - prices[currentDay], maxProfitAfterBuying)` | **Initial Max Profit After Selling** `max(maxProfitAfterBuying + prices[currentDay], maxProfitAfterSelling)` | **Initial Max Profit Before Selling Cooldown** `temp` | <-- **Temp Variable** ` maxProfitAfterSelling` | **Final Max Profit After Selling** |
|---------|--------------------------------------|---------------------------------------|------------------------------------------------|---------------------|--------------------------------------|
|    1    |                 -7                   |                   0                   |                        0                       |                     -                    |                  0                   |
|    2    |                 -1                   |                   0                   |                        0                       |                      0                   |                  0                   |
|    3    |                  0                   |                   4                   |                        0                       |                      0                   |                  0                   |
|    4    |                  0                   |                   4                   |                        0                       |                      4                   |                  0                   |
|    5    |                  3                   |                   7                   |                        4                       |                      4                   |                  7                   |
|    6    |                  3                   |                   7                   |                        4                       |                      7                   |                  7                   |

- On Day 1, the algorithm initializes variables. `maxProfitAfterBuying` is set to -7 (negative of the stock price), `maxProfitAfterSelling` is 0, `maxProfitBeforeSellingCooldown` is 0, and `temp` is not used yet.

- As we iterate through the days, the algorithm updates the variables based on the buying and selling logic, considering the cooldown period. `temp` is used to keep track of the maximum profit one day before selling (cooldown).

- On Day 5, `maxProfitAfterSelling` becomes 7, which is the maximum profit achievable. `temp` is updated to 4.

- The final result is the `maxProfitAfterSelling` on the last day, which is 7.
