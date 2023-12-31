## Solution


Time Complexity: O(N * k) k- number of transactions. 

1. dp1-`previousDayProfit` dp2- `currentDayProfit` and loop through tranaction and days(i.e)`prices`

2. before going into day loop -->Initialize the maximum amount of money that can be spent (negative value as we are buying)

3. Calculate the maximum profit achievable on the current day `currentDayProfit[day] = Math.max(currentDayProfit[day - 1], prices[day] + maxSpentSoFar);` and `maxSpentSoFar`maximum amount of money that can be spent

4. After a day ends previous profit is the current profit and we go to next iteration for each variable.

5. `currentDayProfit[days-1]` is the required result.

Lessons Learnt:
The line System.arraycopy(currentDayProfit, 0, previousDayProfit, 0, days); is used to update the previousDayProfit array with the values of currentDayProfit. This is done at the end of each transaction loop iteration to prepare for the next transaction.

## Code



``` java
    public class Solution {
    public int maxProfit(int maxTransactions, int[] prices) {
        // Check base cases
        int n = prices.length;
        if (n<=1) {
            return 0;
        }
        
        // If the number of allowed transactions is greater than or equal to half of the number of days,
        // then we can perform as many transactions as needed.
        if (maxTransactions >= prices.length / 2) {
            return quickSolve(prices);
        }

        // Length of the prices array
        int days = prices.length;

        // dp1 represents the previous day's profit
        int[] previousDayProfit = new int[days];

        // dp2 represents the current day's profit
        int[] currentDayProfit = new int[days];

        // Perform transactions for each allowed transaction
        for (int transaction = 0; transaction < maxTransactions; transaction++) {
            // Initialize the maximum amount of money that can be spent (negative value as we are buying)
            int maxSpentSoFar = -prices[0];

            // Loop through the days to calculate the profit for the current transaction
            for (int day = 1; day < days; day++) {
                // Calculate the maximum profit achievable on the current day
                currentDayProfit[day] = Math.max(currentDayProfit[day - 1], prices[day] + maxSpentSoFar);

                // Update the maximum amount of money that can be spent (buying) for the next day
                maxSpentSoFar = Math.max(maxSpentSoFar, previousDayProfit[day] - prices[day]);
            }

            // Update dp1 with the values of dp2 for the next transaction
            System.arraycopy(currentDayProfit, 0, previousDayProfit, 0, days);
        }

        // The final result is the maximum profit achievable after all transactions
        return currentDayProfit[days - 1];
    }

    // Helper method for a quick solution when the number of allowed transactions is large
    private int quickSolve(int[] prices) {
        int totalProfit = 0;
        // Loop through the days to calculate the total profit
        for (int day = 1; day < prices.length; day++) {
            // If the current day's price is higher than the previous day, make a profit
            if (prices[day - 1] < prices[day]) {
                totalProfit += prices[day] - prices[day - 1];
            }
        }
        // Return the total profit
        return totalProfit;
    }
}
```

Transaction: 0, Day: 1
maxSpentSoFar: -2
currentDayProfit: [0, 0, 0, 0, 0, 0]
Transaction: 0, Day: 2
maxSpentSoFar: -2
currentDayProfit: [0, 0, 4, 0, 0, 0]
Transaction: 0, Day: 3
maxSpentSoFar: -2
currentDayProfit: [0, 0, 4, 4, 0, 0]
Transaction: 0, Day: 4
maxSpentSoFar: 0
currentDayProfit: [0, 0, 4, 4, 4, 0]
Transaction: 0, Day: 5
maxSpentSoFar: 0
currentDayProfit: [0, 0, 4, 4, 4, 4]
Transaction: 1, Day: 1
maxSpentSoFar: -2
currentDayProfit: [0, 0, 4, 4, 4, 4]
Transaction: 1, Day: 2
maxSpentSoFar: -2
currentDayProfit: [0, 0, 4, 4, 4, 4]
Transaction: 1, Day: 3
maxSpentSoFar: -1
currentDayProfit: [0, 0, 4, 4, 4, 4]
Transaction: 1, Day: 4
maxSpentSoFar: 4
currentDayProfit: [0, 0, 4, 4, 4, 4]
Transaction: 1, Day: 5
maxSpentSoFar: 4
currentDayProfit: [0, 0, 4, 4, 4, 7]
7



## Code to print 

``` java
     import java.util.*;



public class MyClasss {
    public static int maxProfit(int maxTransactions, int[] prices) {
        // Check base cases
        int n = prices.length;
        if (n <= 1) {
            return 0;
        }

        // number of allowed transactions >= number of days/2 quickSolve
        if (maxTransactions >= prices.length / 2) {
            return quickSolve(prices);
        }

        // Length of the prices array
        int days = prices.length;

        // dp1 represents the previous day's profit
        int[] previousDayProfit = new int[days];

        // dp2 represents the current day's profit
        int[] currentDayProfit = new int[days];

        for (int transaction = 0; transaction < maxTransactions; transaction++) {
            // Initialize the maximum amount of money that can be spent (negative value as we are buying)
            int maxSpentSoFar = -prices[0];

            // Loop through the days to calculate the profit for the current transaction
            for (int day = 1; day < days; day++) {
                // Calculate the maximum profit achievable on the current day
                currentDayProfit[day] = Math.max(currentDayProfit[day - 1], prices[day] + maxSpentSoFar);

                // Update the maximum amount of money that can be spent (buying) for the next day
                maxSpentSoFar = Math.max(maxSpentSoFar, previousDayProfit[day] - prices[day]);

                // Print statements for better understanding
                System.out.println("Transaction: " + transaction + ", Day: " + day);
                System.out.println("maxSpentSoFar: " + maxSpentSoFar);
                System.out.println("currentDayProfit: " + Arrays.toString(currentDayProfit));
            }

            // Update dp1 with the values of dp2 for the next transaction
            System.arraycopy(currentDayProfit, 0, previousDayProfit, 0, days);
        }

        // The final result is the maximum profit achievable after all transactions
        return currentDayProfit[days - 1];
    }

    // Helper method for a quick solution when the number of allowed transactions is large
    private static int quickSolve(int[] prices) {
        int totalProfit = 0;
        // Loop through the days to calculate the total profit
        for (int day = 1; day < prices.length; day++) {
            // If the current day's price is higher than the previous day, make a profit
            if (prices[day - 1] < prices[day]) {
                totalProfit += prices[day] - prices[day - 1];
            }
        }
        // Return the total profit
        return totalProfit;
    }
    
    public static void main(String args[]){
        System.out.println(maxProfit(2, new int[]{3, 2, 6, 5, 0, 3}));

        
    }
}

```
