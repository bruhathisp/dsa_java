## Solution

Find minimum number of coins needed to make change for the target amount.


 dp array is used to store the minimum number of coins needed to make change for each amount from 0 to the target amount (amount). 

For each coin, the algorithm iterates through the dp array from coin to amount.

It updates dp[i] if there is a valid solution for the current amount (i) using the current coin.

The goal is to find the minimum number of coins needed to make change for each amount.

dp[i - coin] represents the minimum number of coins needed to make change for the amount i - coin

min(dp[i - coin], d[p]) is the current value 

Time complexity O(numberOfCoins*TotalAmount) 


### Code
``` java
class Solution {
    public int coinChange(int[] coins, int amount) {
        // Create an array to store the fewest number of coins needed for each amount
        int[] dp = new int[amount + 1];

        // Initialize dp array with a value greater than the maximum possible number of coins
        Arrays.fill(dp, Integer.MAX_VALUE - 1);
        
        // The fewest number of coins needed to make change for 0 is 0.
        dp[0] = 0;

        // Update the dp array by considering each coin
        for (int coin : coins) {
            // Loop through the dp array to update values based on the current coin
            for (int i = coin; i <= amount; i++) {
                // Update dp[i] with the minimum ( current value , dp[i - coin] + 1)
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }

        // If dp[amount] remains its initial value, no valid combination was found
        return dp[amount] == Integer.MAX_VALUE - 1 ? -1 : dp[amount];
    }
}
```


| Coin | Current Amount | Old Value                  | New Value | Updated dp          |
|------|-----------------|----------------------------|-----------|----------------------|
|      | 1               | Integer.MAX_VALUE          | 1         | [0, 1, 2, 3, 4]      |
|      | 2               | Integer.MAX_VALUE          | 2         | [0, 1, 2, 3, 4]      |
|      | 3               | Integer.MAX_VALUE          | 3         | [0, 1, 2, 3, 4]      |
|      | 4               | Integer.MAX_VALUE          | 4         | [0, 1, 2, 3, 4]      |
|      |                 |                            |           |                      |
| 2    | 2               | 2                          | 1         | [0, 1, 1, 2, 2]      |
|      | 3               | 3                          | 2         | [0, 1, 1, 2, 2]      |
|      | 4               | 4                          | 2         | [0, 1, 1, 2, 2]      |
|      |                 |                            |           |                      |
| 3    | 4               | 2                          | 1         | [0, 1, 1, 1, 2]      |
|      |                 | 2                          | 2         | [0, 1, 1, 1, 2]      |


![image](https://github.com/bruhathisp/dsa_java/assets/91585301/b744bccb-21e2-46e3-921d-cbb77dc5d5ea)

