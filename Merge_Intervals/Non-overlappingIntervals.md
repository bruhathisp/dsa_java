## Solution




1. **Edge Case Handling**: If there are no intervals (`intervals.length == 0`), it returns `0` since there's nothing to remove.

2. **Sorting Intervals**: The intervals are sorted based on their end times (`a[1] - b[1]`). Sorting by end times helps to ensure that, as we process intervals in sequence, we always have the interval with the earliest end time next, minimizing the potential for future overlaps.

3. **Iterating Through Intervals**:
   - The variable `currentEnd` stores the end time of the last added interval that doesn't overlap with previously added intervals. Initially, it's set to the end time of the first interval (`intervals[0][1]`).
   - For each subsequent interval, the algorithm checks if its start time (`intervals[i][0]`) is greater than or equal to `currentEnd`. If so, this interval doesn't overlap with the previous one, and `currentEnd` is updated to this interval's end time.
   - If an interval's start time is less than `currentEnd`, it overlaps with the previous interval, and the counter `ans` is incremented.

4. **Result**: The variable `ans` holds the count of overlapping intervals, which is the minimum number of intervals to be removed to eliminate all overlaps. This value is returned as the solution.

The approach effectively minimizes the removal of intervals by always choosing the interval that ends the earliest, thereby reducing the chance of future overlaps. The sorting step is critical, as it arranges the intervals in a way that the greedy approach (choosing the interval that ends earliest) can be applied.
``` java
class Solution {
  public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0)
      return 0;

    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

    int ans = 0;
    int currentEnd = intervals[0][1];

    for (int i = 1; i < intervals.length; ++i)
      if (intervals[i][0] >= currentEnd)
        currentEnd = intervals[i][1];
      else
        ++ans;

    return ans;
  }
}
```
