## Solution



The objective is to find the intersections of two lists of closed intervals.

Java Solution:

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> result = new ArrayList<>();
        int i = 0, j = 0;

        while (i < firstList.length && j < secondList.length) {
            // Find the intersection between two intervals
            int startMax = Math.max(firstList[i][0], secondList[j][0]);
            int endMin = Math.min(firstList[i][1], secondList[j][1]);

            // If the intervals overlap, add the intersection to the result
            if (startMax <= endMin) {
                result.add(new int[]{startMax, endMin});
            }

            // Move the pointer that has the smaller end time
            if (firstList[i][1] < secondList[j][1]) {
                i++;
            } else {
                j++;
            }
        }

        // Convert the result list to an array
        return result.toArray(new int[result.size()][]);
    }
}
```

In this solution:

- We iterate through both lists using two pointers (`i` for `firstList` and `j` for `secondList`).
- For each pair of intervals, we calculate the maximum start time (`startMax`) and the minimum end time (`endMin`) to find their intersection.
- If the intersection is valid (start time <= end time), we add it to the `result` list.
- We then move the pointer that points to the interval with the earlier end time.
- Finally, we convert and return the `result` list as an array of intervals.
