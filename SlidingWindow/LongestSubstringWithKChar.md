## Solution

[Question](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/description/)

**Problem Statement**: The problem asks us to find the length of the longest substring in the given string 's' such that each character in that substring appears at least 'k' times.


Explanation:

1. The `longestSubstring` method starts the process from the beginning of the string (`s`) and checks the entire string for the longest valid substring with at least `k` repeating characters.

2. The `longestValidSubstring` method checks if the substring's length is less than `k`. If so, it returns `0` because it's not possible to satisfy the condition.

3. It then counts the frequency of each character in the substring using an array `charCount`.

4. Next, it checks if each character occurs at least `k` times in the substring. If not, it splits the substring at the character where the condition fails and recursively checks the left and right substrings.

5. Finally, if all characters occur at least `k` times, it returns the length of the substring.

```java
public class Solution {
    public int longestSubstring(String s, int k) {
        // Start from the beginning of the string and check the entire string
        return longestValidSubstring(s, k, 0, s.length());
    }
    
    private int longestValidSubstring(String s, int k, int start, int end) {
        // If the substring length is less than k, it can't satisfy the condition
        if (end - start < k) {
            return 0;
        }
        
        // Count the frequency of each character in the substring
        int[] charCount = new int[26]; // Assuming lowercase English letters
        for (int i = start; i < end; i++) {
            charCount[s.charAt(i) - 'a']++;
        }
        
        // Check if each character occurs at least k times in the substring
        for (int i = start; i < end; i++) {
            if (charCount[s.charAt(i) - 'a'] < k) {
                // If not, split the substring at character i
                // and recursively check the left and right substrings
                return Math.max(
                    longestValidSubstring(s, k, start, i), // Left substring
                    longestValidSubstring(s, k, i + 1, end) // Right substring
                );
            }
        }
        
        // If all characters occur at least k times, return the length of the substring
        return end - start;
    }
}
```
