

## Solution

## Lessons Learnt
Time complexity 
I spent a whole day not knowing where I went wrong, but it was because of the `improper initialization or modification of array elements` and 'not defining arrays and initializing them outside the if statement`

1. Instead of `Integer.compare(x,y)`, you can also use `(x,y) -> (x[0]-y[0])`.
2. When you have made too many changes, write down, step back, and see if a different approach helps.
3. In `List<int[]>`, `int[]` is not a static. In Java, arrays can be dynamically created and modified.
4. Inside the loop, when merging intervals, the code directly assigns `intervals[i]` to `currentInt`. This means that `currentInt` is referring to the same array as `intervals[i]`. Consequently, when modifying `currentInt`, it also modifies the original array, leading to incorrect results.
5. Another way of handling the above conflict is to copy the elements of the array.
   ```java
   for (int i = 0; i < intervals.length; i++) {
       if (currentInt == null) {
           currentInt = Arrays.copyOf(intervals[i], intervals[i].length); // Create a copy
       } else if (currentInt[1] >= intervals[i][0]) {
           // Correct: create a new array for currentInt
           currentInt = new int[]{currentInt[0], Math.max(currentInt[1], intervals[i][1])};
       } else {
           result.add(currentInt);
           currentInt = Arrays.copyOf(intervals[i], intervals[i].length); // Create a new copy
       }
   }
   ```

### Method 1

1. Sort it
2. create `currentInt` to keep track of the min and max overlapping interval
3. If `currentInt == null` assign the first interval from `intervals`
4. If the end of `currentInt` is greater than the start of the next interval `intervals[i][0]` then update the end  currentInt[1]
5. Find the max of the end because in a test case like this you end up giving the wrong answer.

Input
intervals =[[1,4],[2,3]]

Output
[[1,3]]

Expected
[[1,4]]

6. If the intervals don't overlap, just add the `currentInt` to the `result` arrayList and update the `currentInt`.
   

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> result = new ArrayList<>();

        Arrays.sort(intervals, (x,y) -> (x[0]-y[0]));

        int n = 0;
        int[] currentInt = null;

        for (int i = 0; i < intervals.length; i++) {
            if (currentInt == null) {
                currentInt = new int[]{intervals[i][0], intervals[i][1]};
                
            } else if (currentInt[1] >= intervals[i][0]) {
                // Merge the intervals if necessary
                currentInt[1] = Math.max(currentInt[1], intervals[i][1]);
            } else {
                result.add(currentInt);
                currentInt = new int[]{intervals[i][0], intervals[i][1]};
            }
        }

        // Handle the last interval after the loop
        if (currentInt != null) {
            result.add(currentInt);
        }

        int[][] resultArray = result.toArray(new int[result.size()][]);
        return resultArray;
    }
}
```

### Method 2

This has a optimised space with the same time complexity.

Note that it doesn't modify the interval array.


`ans.get(ans.size() - 1)[1] = Math.max(interval[1], ans.get(ans.size() - 1)[1]);`


```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
        List<int[]> ans = new ArrayList<>();
        
        for (int[] interval : intervals) {
            if (ans.isEmpty() || interval[0] > ans.get(ans.size() - 1)[1]) {
                ans.add(interval);
            } else {
                ans.get(ans.size() - 1)[1] = Math.max(interval[1], ans.get(ans.size() - 1)[1]);
            }
        }
        
        return ans.toArray(new int[ans.size()][]);
    }
}
```

```
