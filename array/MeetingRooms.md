Meeting Rooms
Given an array of meeting time intervals consisting of start and end times[[s1,e1],[s2,e2],...](si< ei), determine if a person could attend all meetings.
Example 1:
Input:
[[0,30],[5,10],[15,20]]
Output:
 false
Example 2:
Input:
 [[7,10],[2,4]]
​
Output:
 true
Solution & Analysis
The idea here is to sort the meetings by starting time. Then, go through the meetings one by one and make sure that each meeting ends before the next one starts.
Time complexity : O(nlogn). The time complexity is dominated by sorting. Once the array has been sorted, only O(n) time is taken to go through the array and determine if there is any overlap.
Space complexity : O(1). Since no additional space is allocated.

Sort, then compare last.end vs. current.start; similar to Merge Intervals

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public boolean canAttendMeetings(Interval[] intervals) {
        Arrays.sort(intervals, new Comparator<Interval>() {
           public int compare(Interval i1, Interval i2) {
               return i1.start - i2.start;
           } 
        });
        Interval last = null;
        for (Interval i: intervals) {
            if (last != null && i.start < last.end) {
                return false;
            }
            last = i;
        }
        return true;
    }
}
```
Sorting

```java
public boolean canAttendMeetings(Interval[] intervals) {
    Arrays.sort(intervals, new Comparator<Interval>() {
        public int compare(Interval i1, Interval i2) {
            return i1.start - i2.start;
        }        
    });
    for (int i = 0; i < intervals.length - 1; i++) {
        if (intervals[i].end > intervals[i + 1].start) return false;
    }
    return true;
}
```
Other implementation
Sorting - throw expection

```java
private boolean canAttendMeetings(Interval[] intervals) {
    try {
        Arrays.sort(intervals, new IntervalComparator());
    } catch (Exception e) {
        return false;
    }
    return true;
}
​
private class IntervalComparator implements Comparator<Interval> {
    @Override
    public int compare(Interval o1, Interval o2) {
        if (o1.start < o2.start && o1.end <= o2.start)
            return -1;
        else if (o1.start > o2.start && o1.start >= o2.end)
            return 1;
        throw new RuntimeException();
    }
}
```
Java 8
```java
public boolean canAttendMeetings(Interval[] intervals) {
    // Sort the intervals by start time
    Arrays.sort(intervals, (x, y) -> x.start - y.start);
    for (int i = 1; i < intervals.length; i++)
        if (intervals[i-1].end > intervals[i].start)
            return false;
    return true;
}
```



# A custom nested list implementation 
Certainly! Here's an example using the second approach with a `List<List<Integer>>`:

```java
import java.util.ArrayList;
import java.util.List;

public class MeetingScheduler {
    public static void main(String[] args) {
        // Initialize a list of intervals
        List<List<Integer>> intervals = new ArrayList<>();
        intervals.add(List.of(7, 10));
        intervals.add(List.of(2, 4));
        // Add more intervals as needed

        // Call the canAttendMeetings method
        boolean canAttendAllMeetings = canAttendMeetings(intervals);

        // Print the result
        System.out.println("Can attend all meetings? " + canAttendAllMeetings);
    }

    // Method to check if a person can attend all meetings
    public static boolean canAttendMeetings(List<List<Integer>> intervals) {
        intervals.sort((a, b) -> a.get(0) - b.get(0));
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals.get(i - 1).get(1) > intervals.get(i).get(0)) {
                return false;
            }
        }
        return true;
    }
}
```

In this example, `intervals` is a `List<List<Integer>>`, where each inner list represents an interval. The `canAttendMeetings` method takes this list of intervals, sorts them based on the start times, and then checks for any overlapping intervals. The result is printed to the console.

Feel free to add more intervals to the `intervals` list in the `main` method for additional testing.
