## Solution


Lessons Learnt

1. result.add(String.join(".", currentIP));
2. 


#### isValidSegment

The `isValidSegment` method checks whether a given segment of the input string is a valid component for forming an IP address. Here's a breakdown of the conditions:

1. `segment.length() > 1 && segment.charAt(0) == '0'`: This condition checks if the segment has more than one digit and if the first digit is '0'. In IP addresses, leading zeros in a segment are not allowed. For example, "01" is not a valid segment.

2. `int value = Integer.parseInt(segment)`: This line converts the segment (which is assumed to be a string representation of an integer) into an actual integer value.

3. `return value >= 0 && value <= 255;`: This condition ensures that the integer value obtained from the segment is within the valid range for an IP address component, which is between 0 and 255 (inclusive).

Combining these conditions, the method returns `true` if the segment is a valid component and `false` otherwise.


``` java

import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> restoreIPAddresses(String s) {
        List<String> result = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(String s, int start, List<String> currentIP, List<String> result) {
        if (start == s.length() && currentIP.size() == 4) {
            result.add(String.join(".", currentIP));
            return;
        }

        for (int i = 1; i <= 3; i++) {
            if (start + i > s.length()) {
                break;
            }

            String segment = s.substring(start, start + i);
            if (isValidSegment(segment)) {
                // Adding segment and Update IP
                currentIP.add(segment);

                // Backtracking. Removed last segment
                backtrack(s, start + i, currentIP, result);
                currentIP.remove(currentIP.size() - 1);
            }
        }
    }

    private boolean isValidSegment(String segment) {
        if (segment.length() > 1 && segment.charAt(0) == '0') {
            return false; // Leading zeros are not allowed
        }

        int value = Integer.parseInt(segment);
        return value >= 0 && value <= 255;
    }
}


  ```




```
Checking segment: 2, start: 0, i: 1
Adding segment: 2, currentIP: [2]
going to Backtrack and Remove last segment. Current  s  25525511135   start  01  currentIP  [2]   result   []
Checking segment: 5, start: 1, i: 1
Adding segment: 5, currentIP: [2, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  11  currentIP  [2, 5]   result   []
Checking segment: 5, start: 2, i: 1
Adding segment: 5, currentIP: [2, 5, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  21  currentIP  [2, 5, 5]   result   []
Checking segment: 2, start: 3, i: 1
Adding segment: 2, currentIP: [2, 5, 5, 2]
going to Backtrack and Remove last segment. Current  s  25525511135   start  31  currentIP  [2, 5, 5, 2]   result   []
Checking segment: 5, start: 4, i: 1
Adding segment: 5, currentIP: [2, 5, 5, 2, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  41  currentIP  [2, 5, 5, 2, 5]   result   []
Checking segment: 5, start: 5, i: 1
Adding segment: 5, currentIP: [2, 5, 5, 2, 5, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  51  currentIP  [2, 5, 5, 2, 5, 5]   result   []
Checking segment: 1, start: 6, i: 1
Adding segment: 1, currentIP: [2, 5, 5, 2, 5, 5, 1]
going to Backtrack and Remove last segment. Current  s  25525511135   start  61  currentIP  [2, 5, 5, 2, 5, 5, 1]   result   []
Checking segment: 1, start: 7, i: 1
Adding segment: 1, currentIP: [2, 5, 5, 2, 5, 5, 1, 1]
going to Backtrack and Remove last segment. Current  s  25525511135   start  71  currentIP  [2, 5, 5, 2, 5, 5, 1, 1]   result   []
Checking segment: 1, start: 8, i: 1
Adding segment: 1, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1]
going to Backtrack and Remove last segment. Current  s  25525511135   start  81  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 1]   result   []
Checking segment: 3, start: 9, i: 1
Adding segment: 3, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1, 3]
going to Backtrack and Remove last segment. Current  s  25525511135   start  91  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 1, 3]   result   []
Checking segment: 5, start: 10, i: 1
Adding segment: 5, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1, 3, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  101  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 1, 3, 5]   result   []
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1, 3]
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1]
Checking segment: 35, start: 9, i: 2
Adding segment: 35, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1, 35]
going to Backtrack and Remove last segment. Current  s  25525511135   start  92  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 1, 35]   result   []
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 1]
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1]
Checking segment: 13, start: 8, i: 2
Adding segment: 13, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 13]
going to Backtrack and Remove last segment. Current  s  25525511135   start  82  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 13]   result   []
Checking segment: 5, start: 10, i: 1
Adding segment: 5, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 13, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  101  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 13, 5]   result   []
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 13]
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1]
Checking segment: 135, start: 8, i: 3
Adding segment: 135, currentIP: [2, 5, 5, 2, 5, 5, 1, 1, 135]
going to Backtrack and Remove last segment. Current  s  25525511135   start  83  currentIP  [2, 5, 5, 2, 5, 5, 1, 1, 135]   result   []
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 1]
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1]
Checking segment: 11, start: 7, i: 2
Adding segment: 11, currentIP: [2, 5, 5, 2, 5, 5, 1, 11]
going to Backtrack and Remove last segment. Current  s  25525511135   start  72  currentIP  [2, 5, 5, 2, 5, 5, 1, 11]   result   []
Checking segment: 3, start: 9, i: 1
Adding segment: 3, currentIP: [2, 5, 5, 2, 5, 5, 1, 11, 3]
going to Backtrack and Remove last segment. Current  s  25525511135   start  91  currentIP  [2, 5, 5, 2, 5, 5, 1, 11, 3]   result   []
Checking segment: 5, start: 10, i: 1
Adding segment: 5, currentIP: [2, 5, 5, 2, 5, 5, 1, 11, 3, 5]
going to Backtrack and Remove last segment. Current  s  25525511135   start  101  currentIP  [2, 5, 5, 2, 5, 5, 1, 11, 3, 5]   result   []
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 11, 3]
Backtracking. Removed last segment. Current currentIP: [2, 5, 5, 2, 5, 5, 1, 11]
Checking segment: 35, start: 9, i: 2
Adding segment: 35, currentIP: [2, 5, 5, 2, 5, 5, 1, 11, 35]
g.
.
.
.
.
.

```


``` java

  import java.util.ArrayList;
import java.util.List;

public class MyClass {
    public static List<String> restoreIPAddresses(String s) {
        List<String> result = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), result);
        return result;
    }

    private static void backtrack(String s, int start, List<String> currentIP, List<String> result) {
        if (start == s.length() && currentIP.size() == 4) {
    result.add(String.join(".", currentIP));
    return;
}

for (int i = 1; i <= 3; i++) {
    if (start + i > s.length()) {
        break;
    }

    String segment = s.substring(start, start + i);
    System.out.println("Checking segment: " + segment + ", start: " + start + ", i: " + i);

    if (isValidSegment(segment)) {
        currentIP.add(segment);
        System.out.println("Adding segment: " + segment + ", currentIP: " + currentIP);
        
        System.out.println("going to Backtrack and Remove last segment. Current  s  " + s +"   start  "+ start + i +"  currentIP  "+ currentIP+"   result   "+ result);
        backtrack(s, start + i, currentIP, result);
        currentIP.remove(currentIP.size() - 1);
        System.out.println("Backtracking. Removed last segment. Current currentIP: " + currentIP);
    }
}

    }

    private static boolean isValidSegment(String segment) {
        if (segment.length() > 1 && segment.charAt(0) == '0') {
            return false; // Leading zeros are not allowed
        }

        int value = Integer.parseInt(segment);
        return value >= 0 && value <= 255;
    }
    
    public static void main(String args[]){
        
        System.out.println(restoreIPAddresses("25525511135"));
    }
}

```
