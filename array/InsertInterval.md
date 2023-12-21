## Solution

I came up with this by referring to the Meeting Rooms sum, but it is different because you have to merge the interval and we are working with arrays here rather than the custom Interval Data Structure(in Meeting Rooms).

Lessons Learnt:
1. In Java, `arrays` do not have a `start` property or method. Instead, you access array elements by `index`. For example, if you have an array intervals, you access the first element using `intervals[0]`, the second element using intervals[1], and so on.
2. When printing Nested Arrays use `deepToString` instead of `toString`. For instance, `System.out.println(Arrays.deepToString(intervals));`
3. Conversion to List:  To use `add` method
   
The use of `List<int[]>` is convenient when dynamically adding intervals. **Arrays in Java have fixed sizes**, and using a **list allows for flexibility** when dealing with a variable number of intervals.
4. Since we have the result in `List<int[]>` type we need to convert it to `array` again. so we use `int[][] resultArray = result.toArray(new int[result.size()][]);`

`new int[result.size()][]` creates a new 2D array of integers with the same number of rows as the size of the list (result.size()). Each row will correspond to an int[] in the list.

5. `(x, y) -> Integer.compare(x[0], y[0]):` This is a lambda expression representing a comparator. It's defining how to compare two elements x and y from the array during the sorting process.

x and y are two subarrays (arrays of integers) from resultArray.

x[0] and y[0] are the first elements (values in the first column) of the subarrays. It's comparing the values in the first column of each subarray.



## Tescases

Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]

## Method 1   

create a dynamic array--> use if statement and find overlap--> find the min start and max_end--> 

add the updated `newInterval` to the dynamic array--> convert the dynamic array to normal array--> sort and return


Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

So I wrote down all the scenarios an interval can overlap another and kept as a condition for the if statement
1. [3,5] and [4,8]
2. [6,7] and [4,8] (like a subset)
3. [8,10] and [4,8]

Next, I found the minimum of start and maximum of end values to find the merged interval.

I gave a print statement for checking the interval and I learnt a lot about the syntax of Arrays and Nested Arrays in java.



``` java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {

        List<int[]> result = new ArrayList<>();
        for (int i = 0; i < intervals.length; i++) {

        if ((intervals[i][1] >= newInterval[0] && intervals[i][0] <= newInterval[1])
    || (intervals[i][0] >= newInterval[1] && intervals[i][1] <= newInterval[0])
    || (intervals[i][1] >= newInterval[0] && intervals[i][0] <= newInterval[0])) {

    int min_start = Math.min(intervals[i][0], newInterval[0]);
    int max_end = Math.max(intervals[i][1], newInterval[1]);

    newInterval[0] = min_start;
    newInterval[1] = max_end;
}
else{

result.add(intervals[i]);

}

}

        System.out.println(Arrays.toString(newInterval));
        System.out.println(Arrays.deepToString(intervals));

        result.add( newInterval);

        // result.toArray(new int[result.size()][]);

        int[][] resultArray = result.toArray(new int[result.size()][]);


        Arrays.sort(resultArray, (x,y) -> Integer.compare(x[0], y[0]));

        return resultArray;
}
}
```

# Method 2

The exact implementation in much cleaner code 

``` java
public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
    List<Interval> result = new LinkedList<>();
    int i = 0;
    // add all the intervals ending before newInterval starts
    while (i < intervals.size() && intervals.get(i).end < newInterval.start)
        result.add(intervals.get(i++));
    // merge all overlapping intervals to one considering newInterval
    while (i < intervals.size() && intervals.get(i).start <= newInterval.end) {
        newInterval = new Interval( // we could mutate newInterval here also
                Math.min(newInterval.start, intervals.get(i).start),
                Math.max(newInterval.end, intervals.get(i).end));
        i++;
    }
    result.add(newInterval); // add the union of intervals we got
    // add all the rest
    while (i < intervals.size()) result.add(intervals.get(i++)); 
    return result;
}
```
