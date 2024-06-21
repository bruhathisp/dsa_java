[Question](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

### Approach:

1. **Initialization:**
   - Use two pointers, `left` and `right`, to represent the current window.
   - Use an array `count` to store the frequency of each character in the current window.
   - Use `maxCount` to store the count of the most frequent character in the current window. (right)
   - Initialize `maxLength` to keep track of the maximum length of the valid window found so far.

2. **Expand the Window:**
   - Move the `right` pointer to the right, expanding the window and updating the frequency count of the current character.
   - Update `maxCount` with the maximum frequency of any character in the current window.

3. **Condition Check:** For the current window to be valid, the number of replacements needed should be less than or equal to `k`. This can be checked by the formula:
   `number of replacements= window size - count of the most frequent character`
   **That is**
   `number of replacements = right-left + 1 - maxCount`

5. **Shrink the Window:** If the number of replacements exceeds `k`, it means the current window is invalid. To make the window valid again, you need to shrink the window from the left by moving the `left` pointer to the right. This involves:
   - Decrementing the frequency count of the character at the `left` pointer.
   - Moving the `left` pointer one step to the right.
   - Rechecking the condition with the new window size.

   - If the number of characters to be replaced (i.e., the total length of the window minus the count of the most frequent character) exceeds `k`, move the `left` pointer to the right to shrink the window until the condition is satisfied.

6. **Update the Result:**
   - Keep track of the maximum length of the valid window found during the process.

### Java Implementation:

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int n = s.length();
        int[] count = new int[26];
        int left = 0, maxCount = 0, maxLength = 0;

        for (int right = 0; right < n; right++) {
            count[s.charAt(right) - 'A']++;
            maxCount = Math.max(maxCount, count[s.charAt(right) - 'A']);

            // If the current window size minus the count of the most frequent character is greater than k, shrink the window
            while (right - left + 1 - maxCount > k) {
                count[s.charAt(left) - 'A']--;
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

### Explanation:

1. **Initialization:**
   - `int[] count = new int[26];` — Array to store the frequency of each character in the current window.
   - `int left = 0, maxCount = 0, maxLength = 0;` — Initialize pointers and variables.

2. **Expand the Window:**
   - Iterate through the string with the `right` pointer.
   - Update the frequency count of the current character.
   - Update `maxCount` with the maximum frequency of any character in the current window.

3. **Shrink the Window:**
   - If the current window size minus the count of the most frequent character is greater than `k`, shrink the window by moving the `left` pointer to the right and updating the frequency count.

4. **Update the Result:**
   - Update `maxLength` with the maximum length of the valid window found.

This approach ensures that we find the longest substring with the same letter after performing at most `k` replacements in linear time, O(n), making it efficient for large input sizes.
