## Solution

#### Steps Time complexity O(m*n)
1. `i <= tar`  iterate through this
2. `numi : num` the iterate through each element in num
3. Only when `i <= tar` is true, update `dp[i]` as `dp[i]+dp[i-numi]` i.e add the last element.
4. The above step is the same as `target - candidates[i] >= 0`
5. After the iteration return the dp[tar]



### Difference between 1 2 and 4 
Let's differentiate Combination Sum I, Combination Sum II, and the new problem (Combo Sum 3) based on the problem statements and solutions.

### Combination Sum I:

**Problem Statement:**
Given an array of distinct integers, find all unique combinations of integers from the array whose sum is equal to the target value.

**Handling Duplicates:**
- The input array can contain duplicate elements.
- The result should not have duplicate combinations.

**Example:**
Input: [2, 3, 6, 7], target: 7
Combinations: [[2, 2, 3], [7]]

### Combination Sum II:

**Problem Statement:**
Given an array of integers, find all unique combinations of integers from the array whose sum is equal to the target value.

**Handling Duplicates:**
- The input array can contain duplicate elements.
- The result may have duplicate combinations if the input array has duplicate elements.

**Example:**
Input: [10, 1, 2, 7, 6, 1, 5], target: 8
Combinations: [[1, 1, 6], [1, 2, 5], [1, 7], [2, 6]]
Note: [1, 1, 6] and [1, 2, 5] are distinct combinations even though they have duplicate elements.

### Combo Sum 3:

**Problem Statement:**
Given an array of distinct integers, find the number of different ways of selecting the elements from the array such that the sum of chosen elements is equal to the target number.

**Handling Duplicates:**
- The input array contains distinct elements.
- The result involves counting the number of ways, and two ways are considered the same if their contents are equal.

**Example:**
Input: [1, 2, 5], target: 5
Ways: (1,1,1,1,1), (1,1,1,2), (1,2,1,1), (2,1,1,1), (1,1,2,1), (2,2,1), (1,2,2), (2,1,2), (5)
Output: 9

In summary:
- Combination Sum I and II focus on finding combinations with a specific sum.
- Combo Sum 3 focuses on counting the number of ways to achieve a specific sum.

### Solution Code for Combo Sum 3:

```java
import java.util.*;

public class Solution {
    public static int findWays(int num[], int tar) {
        int[] dp = new int[tar + 1];
        dp[0] = 1;

        for (int i = 0; i <= tar; ++i)
            for (final int numi : num)
                if (i >= numi)
                    dp[i] += dp[i - numi];

        return dp[tar];
    }
}
```

```
[1, 0, 0, 0, 0, 0]
i  0
numi  1
numi  2
numi  5
[1, 0, 0, 0, 0, 0]
i  1
numi  1
i >= numi
numi1dp[i]1  i   1
numi  2
numi  5
[1, 1, 0, 0, 0, 0]
i  2
numi  1
i >= numi
numi1dp[i]1  i   2
numi  2
i >= numi
numi2dp[i]2  i   2
numi  5
[1, 1, 2, 0, 0, 0]
i  3
numi  1
i >= numi
numi1dp[i]2  i   3
numi  2
i >= numi
numi2dp[i]3  i   3
numi  5
[1, 1, 2, 3, 0, 0]
i  4
numi  1
i >= numi
numi1dp[i]3  i   4
numi  2
i >= numi
numi2dp[i]5  i   4
numi  5
[1, 1, 2, 3, 5, 0]
i  5
numi  1
i >= numi
numi1dp[i]5  i   5
numi  2
i >= numi
numi2dp[i]8  i   5
numi  5
i >= numi
numi5dp[i]9  i   5
[1, 1, 2, 3, 5, 9]
[1, 1, 2, 3, 5, 9]
Number of ways: 9


```


``` java
    import java.util.*;

public class MyClass {
    public static int findWays(int num[], int tar) {
        int[] dp = new int[tar + 1];
        dp[0] = 1;
        System.out.println(Arrays.toString(dp));
        for (int i = 0; i <= tar; ++i) {
            System.out.println("i  " +i);
            
            for (final int numi : num) {
                
                System.out.println("numi  " +numi);
                if (i >= numi) {
                    System.out.println("i >= numi");
                    dp[i] += dp[i - numi];
                    System.out.println("numi"  +numi+  "dp[i]"+dp[i]+"  i   "+i);
                }
            }

            // Print dp array after each iteration
            System.out.println(Arrays.toString(dp));
        }

        // Print final dp array
        System.out.println(Arrays.toString(dp));

        return dp[tar];
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 5};
        int target = 5;
        System.out.println("Number of ways: " + findWays(nums, target));
    }
}


```
