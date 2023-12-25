## Solution

Input 
Gas 1 2  3 4 5 4 1 1 8

Cost 3 4 5 1 2 1 2 10 1 

Ooutput 8

Method 

1. Find if the `sum=gas - cost` is positive or negative.
2. Update `total += sum` if the sum is negative.
3. If `sum` is negative update `sum = 0` and `position = i + 1`.
4. So the `total`, `sum=0` and `position` is not updated, when the `sum` is positive, i.e. the `sum` is started fresh when it is negative and the `sum` gets added up when it is positive. So after when a negative `sum` is encountered, the accumulated `sum` is added with total. Again the sum is set to 0.
5. At last when the `total` is positive it returns the `position`. Else -1 is returned.
   
Keypoints:

   Do not update `position` when `sum` is positive.

   Change `sum=0` when sum is negative.

   Accumulate `sum` when sum is positive.

   


Let's create a table to track the values of `i`, `sum`, `total`, and `position` for each iteration:


| i   | gas | cost | sum  | total | position |
| --- | --- | ---- | ---- | ----- | -------- |
| 0   | 1   | 3    | -2   | 0     | 1        |
| 1   | 2   | 4    | -2   | -2    | 2        |
| 2   | 3   | 5    | -2   | -4    | 3        |
| 3   | 4   | 1    | 3    | -4    | 4        |
| 4   | 5   | 2    | 6    | -4    | 4        |
| 5   | 4   | 1    | 9    | -4    | 4        |
| 6   | 1   | 2    | 8    | -4    | 4        |
| 7   | 1   | 10   | -1   | -5    | 8        |
| 8   | 8   | 1    | 7    | -5    | 8        |

At the end of the iteration, we add the final `sum` to `total`:

- `total += 7` → `total = 2`

Since `total` is positive (`2`), the algorithm returns `8`, which is the expected output. The starting index `8` represents the position from where we can complete the circular journey without running out of gas.




Similarly consider this testcase:

Certainly! Let's create a table to track the values of `i`, `sum`, `total`, and `position` for each iteration:

| i   | gas | cost | sum  | total | position |
| --- | --- | ---- | ---- | ----- | -------- |
| 0   | 1   | 3    | -2   | 0     | 1        |
| 1   | 2   | 4    | -2   | -2    | 2        |
| 2   | 3   | 5    | -2   | -4    | 3        |
| 3   | 3   | 1    | 2    | -4    | 3        |

At the end of the iteration, we add the final `sum` to `total`:

- `total += 2` → `total = -2`

The final step is to check if `total` is greater than or equal to 0. In this case, since `total` is negative (`-2`), the algorithm returns `-1`, indicating that there is no valid starting index to complete the circular journey without running out of gas.


``` java

class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int total = 0;
        int sum = 0;
        int position = 0;

        for (int i = 0; i < gas.length; i++) {
            sum += gas[i] - cost[i];
            if (sum < 0) {
                // when sum is negative 
                total += sum;
                sum = 0;  
                position = i + 1;
            }
        }

        
        // lastly total is updated for positive value
        total += sum;

        return total >= 0 ? position : -1;
    }
}


```
