# Find All Anagrams in a String

### **Cornell Notes**

* * *

  

**Topic:** Algorithm for finding all start indices of anagrams in a string using a sliding window approach.

  

**Date:** May 23, 2024

  

**Source:** Coding Problem - LeetCode #438

* * *

  

**Main Ideas:**

  

*   **Sliding Window Technique:** Utilize a fixed-size sliding window to traverse the longer string `s` to find substrings that are anagrams of `p`.
*   **Character Count Comparison:** Use arrays to count occurrences of each character in `p` and compare with windows in `s`.

* * *

  

**Questions:**

  

*   What are the time and space complexities of this approach?
*   How does the sliding window technique optimize the search for anagrams?

* * *

  

**Notes:**

1\. **Objective:**

  

*   Find all indices in `s` where the substring is an anagram of `p`.

  

1. **Implementation Steps:**
    *   **Check Length:** Return empty if `p.length() > s.length()`.
    *   **Set Up Counts:** Create two count arrays `pCount` and `sCount` for `p` and `s` respectively.
    *   **Initial Window:** Populate `sCount` with the first window (first `p.length()` characters of `s`).
    *   **Sliding Window:**
        *   Slide the window one character at a time.
        *   Update `sCount` by adding the new character and removing the old character.
        *   Compare `sCount` with `pCount` at each step.
    *   **Store Results:** If `pCount` matches `sCount`, store the starting index of the window.
2. **Edge Cases:**
    *   `p` longer than `s`.
    *   Empty strings.
3. **Efficiency:**
    *   **Time Complexity:** O(n) - where n is the length of `s`.
    *   **Space Complexity:** O(1) - constant space for the 26-length count arrays.
4. **Practical Considerations:**
    *   Always verify that character counts match before adding the index to the result.
    *   Consider the impact of large input sizes on performance.

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        // Early exit if `p` is longer than `s`
        if (p.length() > s.length()) return result;

        // Arrays to count frequency of characters in `p` and the current window in `s`
        int[] pCount = new int[26];
        int[] sCount = new int[26];
        
        // Initialize the counts for `p` and the first window of `s`
        for (int i = 0; i < p.length(); i++) {
            pCount[p.charAt(i) - 'a']++;
            sCount[s.charAt(i) - 'a']++;

//"cbaebabacd", p = "abc"  scount[1,1,1,0,.....]  ||    p===== abc  p[1,1,1,000000]
        } 3-3

        // Compare the first window
        if (Arrays.equals(pCount, sCount)) {
            result.add(0);   result=[0,]   ==> result[0,6]
        }

        // Sliding the window over the string `s`
        for (int i = p.length(); i < s.length(); i++) {
            // Include a new character in the window
            sCount[s.charAt(i) - 'a']++;
            // Remove the character going out of the window
            sCount[s.charAt(i - p.length()) - 'a']--;
            // Check if current window is an anagram of `p`
            if (Arrays.equals(pCount, sCount)) {
                result.add(i - p.length() + 1);  3-3+1 =1
            }  result [0,6]
        }

        return result;
    }
}
```

* * *

  

**Summary:**

The algorithm uses a sliding window equal to the length of `p` to scan through `s`, updating and comparing two character count arrays in constant time. As it slides, it adjusts counts for entering and exiting characters. If counts match, it records the start index. This method operates in O(n) time and uses constant space, providing an efficient solution.

* * *
