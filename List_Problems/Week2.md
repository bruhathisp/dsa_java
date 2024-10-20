# Problems

- [Day1](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week2.md#day-1)


# Day 1

Here are the problem links and solutions for the mentioned problems:

---

### 1. **Group Anagrams**
- **LeetCode Link**: [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

#### Solution Explanation:
Group strings that are anagrams of each other. This can be done by categorizing strings based on a sorted version of the string.

#### Approach:
- Iterate over each string.
- Sort the string to use as a key in a hash map.
- Group all strings with the same sorted key together.

#### Time Complexity: 
- **O(n * k log k)**, where *n* is the number of strings and *k* is the maximum length of a string (due to sorting each string).
  
#### Space Complexity: 
- **O(nk)**, for storing the result.

#### Java Solution:
```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String sorted = new String(chars);
            map.computeIfAbsent(sorted, x -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```

---

### 2. **Longest Consecutive Sequence**
- **LeetCode Link**: [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

#### Solution Explanation:
Find the length of the longest consecutive elements sequence in an unsorted array.

#### Approach:
- Use a hash set for constant time lookups.
- Iterate over the array, and for each element, check if it's the start of a sequence (i.e., no preceding element).
- If it's the start, count how many consecutive elements follow.

#### Time Complexity: 
- **O(n)**, as each number is visited only once.
  
#### Space Complexity: 
- **O(n)**, for the hash set.

#### Java Solution:
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        
        int longestStreak = 0;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;
                
                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }
                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }
        return longestStreak;
    }
}
```

---

### 3. **Contains Duplicate II**
- **LeetCode Link**: [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

#### Solution Explanation:
Check if there are duplicates within a given range *k* in the array.

#### Approach:
- Use a sliding window with a hash set to track elements in the current window of size *k*.
- If a duplicate is found in the window, return `true`.

#### Time Complexity: 
- **O(n)**, where *n* is the number of elements.

#### Space Complexity: 
- **O(k)**, for the sliding window.

#### Java Solution:
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < nums.length; i++) {
            if (i > k) {
                set.remove(nums[i - k - 1]);
            }
            if (!set.add(nums[i])) {
                return true;
            }
        }
        return false;
    }
}
```

---

### 4. **Intersection of Two Arrays II**
- **LeetCode Link**: [Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

#### Solution Explanation:
Find the intersection of two arrays, where each element in the result should appear as many times as it appears in both arrays.

#### Approach:
- Use a hash map to count the frequency of elements in one array.
- For the second array, check the count in the map and build the result accordingly.

#### Time Complexity: 
- **O(n + m)**, where *n* and *m* are the lengths of the two arrays.
  
#### Space Complexity: 
- **O(min(n, m))**, for storing the frequency map.

#### Java Solution:
```java
import java.util.*;

class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        List<Integer> result = new ArrayList<>();
        for (int num : nums2) {
            if (map.getOrDefault(num, 0) > 0) {
                result.add(num);
                map.put(num, map.get(num) - 1);
            }
        }
        
        int[] intersection = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            intersection[i] = result.get(i);
        }
        return intersection;
    }
}
```

---

Each problem has its respective solution with time and space complexity discussed. Let me know if you'd like further explanations or alternative approaches!
