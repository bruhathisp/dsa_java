## Solution

[Question](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/1183307437/)

Note in question the constraints are s consists of English letters, digits, symbols and spaces.

## Explanation 

(see the most k characters)

When we write charCount[currentChar], it directly indexes the array at the ASCII value of the character currentChar. This approach assumes that the characters encountered in the string are ASCII characters and uses their ASCII values as indices in the array.


``` java
  class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.isEmpty()) {
            return 0; // If the string is empty, return 0
        }
        
        int[] charCount = new int[256]; // Assuming ASCII characters
        int distinctCount = 0, maxLength = 0;
        for (int left = 0, right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);
            if (charCount[currentChar] == 0) {
                distinctCount++;
            }
            charCount[currentChar]++;

            while (distinctCount < right - left + 1) {
                char charLeft = s.charAt(left);
                charCount[charLeft]--;
                if (charCount[charLeft] == 0) {
                    distinctCount--;
                }
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}


```
