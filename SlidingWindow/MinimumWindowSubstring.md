[Question](https://leetcode.com/problems/minimum-window-substring/)


To solve the "Minimum Window Substring" problem efficiently, we can use the sliding window technique combined with a frequency count. The idea is to expand and shrink the window dynamically while keeping track of the characters needed to form the required substring.

**Notes HashMap<key, value>   -  map.get(key)     -     map.put(key,value)  -   map.getOrDefault(key,0)  -   map.get(key).intValue()**

map.get(c) retrieves the value associated with key c from the map. Since map is a Map<Character, Integer>, map.get(c) returns an Integer object. Now, .intValue() is a method of the Integer class in Java. It returns the integer value represented by this Integer instance. Essentially, it extracts the primitive int value from the Integer object.

**ans[0] -length of the smallest window;  ans[1]-start of window;  ans[2]-end of window**

### Approach

1. **Character Count Hashmap**:
   - Characters are unique in the context of the alphabet (both lowercase and uppercase), making them suitable identifiers.
   - Use a hashmap `dictT` to store the frequency of characters in `t`. `dicT<Charcter,Interger>` 
   - Use toCharString() function `char[] carray= t.toCharString()` 

2. **Sliding Window**:
   - Expand the window by moving the `right` pointer to the right, adding characters to the window.
   - **Shink the Window:**The condition checks if either:
   - 1. No valid window (ans[0] == -1), meaning this is the first valid window found.
     2. The current window length is smaller than the previously recorded smallest window (right - left + 1 < ans[0]).

3. **Track the Minimum Window**:
   - Keep track of the minimum window size and its starting index.
   - Update the result when a valid window (containing all characters of `t`) is found.

### Implementation

Here is the Java implementation:

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public String minWindow(String s, String t) {
        if (s == null || s.length() == 0 || t == null || t.length() == 0) {
            return "";
        }

        // Dictionary to keep a count of all the unique characters in t
         // because the string consists of both small case and upper case using arrays is complicated
        Map<Character, Integer> dictT = new HashMap<>();
        for (char c : t.toCharArray()) {
            dictT.put(c, dictT.getOrDefault(c, 0) + 1);
        }

        // Number of unique characters in t that need to be present in the desired window
        int required = dictT.size();

        // Left and right pointer
        int l = 0, r = 0;

        // Formed is used to keep track of how many unique characters in t
        // are present in the current window in its desired frequency
        int formed = 0;

        // Dictionary to keep a count of all the unique characters in the current window
        Map<Character, Integer> windowCounts = new HashMap<>();

        // ans list of the form (window length, left, right)
        int[] ans = {-1, 0, 0};

        while (r < s.length()) {
            // Add one character from the right to the window
            char c = s.charAt(r);
            windowCounts.put(c, windowCounts.getOrDefault(c, 0) + 1);

            // If the frequency of the current character added equals to the
            // desired count in t then increment the formed count by 1
            if (dictT.containsKey(c) && windowCounts.get(c).intValue() == dictT.get(c).intValue()) {
                formed++;
            }

            // Try and contract the window till the point where it ceases to be 'desirable'
            while (l <= r && formed == required) {
                c = s.charAt(l);
                // Save the smallest window until now
                if (ans[0] == -1 || r - l + 1 < ans[0]) {
                    ans[0] = r - l + 1;
                    ans[1] = l;
                    ans[2] = r;
                }

                // The character at the position pointed by the
                // `left` pointer is no longer a part of the window
                windowCounts.put(c, windowCounts.get(c) - 1);
                if (dictT.containsKey(c) && windowCounts.get(c).intValue() < dictT.get(c).intValue()) {
                    formed--;
                }

                // Move the left pointer ahead, this would help to look for a new window
                l++;
            }

            // Keep expanding the window once we are done contracting
            r++;   
        }

        return ans[0] == -1 ? "" : s.substring(ans[1], ans[2] + 1);
    }
}
```

### Explanation

1. **Initialization**:
   - 
   - Use variables `l` and `r` to represent the left and right pointers of the sliding window.
   - Use `formed` to keep track of how many unique characters in `t` are present in the current window in their required frequency.
   - Use `windowCounts` to keep track of character counts in the current window.

2. **Expand the Window**:
   - Move the `right` pointer to expand the window and update the counts in `windowCounts`.
   - If the current character's frequency matches the required frequency in `t`, increment `formed`.

3. **Shrink the Window**:
   - When all characters are present in the required frequency (`formed == required`), try to shrink the window from the left.
   - Update the result if a smaller valid window is found.
   - Decrease the count of the character at the `left` pointer in `windowCounts` and move the `left` pointer to the right.

4. **Return the Result**:
   - If a valid window is found, return the substring. Otherwise, return an empty string.

### Complexity

- **Time Complexity**: O(m + n), where m is the length of `s` and n is the length of `t`. Both the left and right pointers traverse the string `s` at most once.
- **Space Complexity**: O(m + n), due to the hashmaps used to store the frequency of characters.

This approach ensures that we find the minimum window substring efficiently, even for large input sizes.
