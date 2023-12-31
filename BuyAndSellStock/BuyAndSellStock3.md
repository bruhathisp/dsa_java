## Solution

Dynamic Programming - O(n) Time Complexity


Lessons Learnt:
1. Do not evaluate expressions inside print statement.

Let's break down how the two-pass approach works for calculating the maximum profit:

### Overview:  
1. Buying pass: Start at the end and find `maxPricesofar` and calculate profit[i](profit of this day) by max(todayprofit, tomorrowprofit) `max(profit[i + 1], maxPriceSofar - prices[i])` 
2. Selling pass: Start at the begining and find `minPricesofar`    and calculate profit[i](profit of this day) by max(yesterdayprofit, todayprofit) `max(profit[i - 1], profit[i] + prices[i] - minPriceSofar);` 
3. Atlast calculate `maxProfitSofar`

1. **First Pass (Selling):**
   - Start from the last day (day n-1) and move towards the first day (day 0).
   - For each day, calculate the maximum price (sofar) from the current day until the last day.
   - Calculate the profit for selling on or after each day.
   - Store the profit values in an array (`profit`).

2. **Second Pass (Buying):**
   - Start from the first day (day 0) and move towards the last day (day n-1).
   - For each day, calculate the minimum price (sofar) from the first day until the current day.
   - Calculate the profit for buying on or before each day, considering the profit obtained in the first pass.
   - Update the maximum profit if a higher profit is found.

### Example:

Consider the input array `prices = [1, 3, 1, 2, 4, 8]`.

#### First Pass (Selling):
- Start from the last day (day 5).
  - `maxPrice` is initially set to the last element, so `maxPrice = 8`.
  - Calculate profit for each day: `[0, 0, 0, 0, 4, 0]` (profit for selling on or after each day).

#### Second Pass (Buying):
- Start from the first day (day 0).
  - `minPrice` is initially set to the first element, so `minPrice = 1`.
  - Update profit for buying on or before each day: `[0, 7, 7, 6, 4, 0]`.
  - The maximum profit is found during this pass, which is `9`.

So, the final maximum profit is `9`, and the buying and selling operations are optimized by considering both the buying and selling passes. The two-pass approach ensures that each transaction is considered separately, and the overall maximum profit is calculated.

``` java

    public class Solution {
    public static int maxProfit(int[] prices) {
        // Get the length of the prices array
        int n = prices.length;

        // If there are 0 or 1 elements, there can be no profit
        if (n <= 1) {
            return 0;
        }

        // Array to store the maximum profit achievable by selling on or after each day
        int[] profit = new int[n];

        // Initialize the maximum price sofar as the last element in the prices array
        int maxPriceSofar = prices[n - 1];

        // First pass: Calculate maximum profit by selling on or after each day
        for (int i = n - 2; i >= 0; i--) {
            // Update the maximum price sofar
            maxPriceSofar = Math.max(maxPriceSofar, prices[i]);

            // Calculate the profit achievable on this day and update the profit array
            profit[i] = Math.max(profit[i + 1], maxPriceSofar - prices[i]);
        }

        // Initialize the minimum price sofar as the first element in the prices array
        int minPriceSofar = prices[0];

        // Initialize the maximum profit sofar as the profit achievable on the first day
        int maxProfitSofar = profit[0];

        // Second pass: Calculate maximum profit by buying on or before each day
        for (int i = 1; i < n; i++) {
            // Update the minimum price sofar
            minPriceSofar = Math.min(minPriceSofar, prices[i]);

            // Calculate the profit achievable on this day and update the profit array
            profit[i] = Math.max(profit[i - 1], profit[i] + prices[i] - minPriceSofar);

            // Update the maximum profit sofar
            maxProfitSofar = Math.max(maxProfitSofar, profit[i]);
        }

        // Return the final maximum profit achieved
        return maxProfitSofar;
    }
}
```

Output

First Pass (Selling):


Day 4: Max Price: 8

Day 4: Profit: [0, 0, 0, 0, 4, 0]

Day 3: Max Price: 8

Day 3: Profit: [0, 0, 0, 6, 4, 0]

Day 2: Max Price: 8

Day 2: Profit: [0, 0, 7, 6, 4, 0]

Day 1: Max Price: 8

Day 1: Profit: [0, 7, 7, 6, 4, 0]

Day 0: Max Price: 8

Day 0: Profit: [7, 7, 7, 6, 4, 0]

Second Pass (Buying):


Day 1: MaxPrice=8, Profit[1]=7

Day 1: profit: 9

Day 1: prices[i]: 3

Day 1: minPrice: 1

Day 1: profit[i] + prices[i] - minPrice: 5

Day 1: profit[i - 1]: 7

Day 1: maxProfit9

[7, 9, 7, 6, 4, 0]


Day 2: MaxPrice=8, Profit[2]=7

Day 2: profit: 9

Day 2: prices[i]: 1

Day 2: minPrice: 1

Day 2: profit[i] + prices[i] - minPrice: 7

Day 2: profit[i - 1]: 9

Day 2: maxProfit9

[7, 9, 9, 6, 4, 0]


Day 3: MaxPrice=8, Profit[3]=6

Day 3: profit: 9

Day 3: prices[i]: 2

Day 3: minPrice: 1

Day 3: profit[i] + prices[i] - minPrice: 6

Day 3: profit[i - 1]: 9

Day 3: maxProfit9

[7, 9, 9, 9, 4, 0]


Day 4: MaxPrice=8, Profit[4]=4

Day 4: profit: 9

Day 4: prices[i]: 4

Day 4: minPrice: 1

Day 4: profit[i] + prices[i] - minPrice: 4

Day 4: profit[i - 1]: 9

Day 4: maxProfit9

[7, 9, 9, 9, 9, 0]


Day 5: MaxPrice=8, Profit[5]=0

Day 5: profit: 9

Day 5: prices[i]: 8

Day 5: minPrice: 1

Day 5: profit[i] + prices[i] - minPrice: 0

Day 5: profit[i - 1]: 9

Day 5: maxProfit9

[7, 9, 9, 9, 9, 9]


Final Max Profit: 9



``` java
      import java.util.*;

public class MyClass {
    public static int maxProfit(int[] prices) {
        int n = prices.length;

        if (n <= 1) {
            return 0;
        }

        int[] profit = new int[n];

        int maxPrice = prices[n - 1];

        // First Pass (Selling)
        System.out.println("First Pass (Selling):");
        for (int i = n - 2; i >= 0; i--) {
            maxPrice = Math.max(maxPrice, prices[i]);
            System.out.println("Day " + i + ": Max Price: " + maxPrice);
            profit[i] = Math.max(profit[i + 1], maxPrice - prices[i]);
            System.out.println("Day " + i + ": Profit: " + Arrays.toString(profit));
        }

        // Second Pass (Buying)
        int minPrice = prices[0];
        int maxProfit = profit[0];

        System.out.println("\nSecond Pass (Buying):");
        for (int i = 1; i < n; i++) {
            System.out.println("Day " + i + ": MaxPrice=" + maxPrice + ", Profit[" + i + "]=" + profit[i]);

            minPrice = Math.min(minPrice, prices[i]);
            profit[i] = Math.max(profit[i - 1], profit[i] + prices[i] - minPrice);

            // Update maxProfit if needed
            maxProfit = Math.max(maxProfit, profit[i]);

            System.out.println("Day " + i + ": profit: " + profit[i]);
            System.out.println("Day " + i + ": prices[i]: " + prices[i]);
            System.out.println("Day " + i + ": minPrice: " + minPrice);
            int some =(profit[i] - prices[i] - minPrice);
            System.out.println("Day " + i + ": profit[i] + prices[i] - minPrice: " + some);
            
            System.out.println("Day " + i + ": profit[i - 1]: " + profit[i - 1]);
            
            System.out.println("Day " + i + ": maxProfit" + maxProfit);
            System.out.println(Arrays.toString(profit));
        }

        // Print the final result
        System.out.println("Final Max Profit: " + maxProfit);

        return maxProfit;
    }

    public static void main(String[] args) {
        int[] prices = { 1, 3, 1, 2, 4, 8 };
        maxProfit(prices);
    }
}


```
