## Solution

[Question](https://www.codingninjas.com/studio/problems/longest-substring-with-at-most-k-distinct-characters_2221410?leftPanelTabValue=PROBLEM)

You are given ‘str’ = ‘abbbbbbc’ and ‘K’ = 2, then the substrings that can be formed are [‘abbbbbb’, ‘bbbbbbc’]. Hence the answer is 7.


   - Our goal is to find the length of the longest substring in `str` that contains at most `k` different characters.
   - If the number of distinct characters in the substring exceeds `k`, we'll shrink the window from the left until it's valid again.

2. **Code Explanation**:
   - We initialize variables to keep track of the count of unique characters (`uniqueCount`), the left pointer of the window (`left`), and the maximum length of the valid substring (`maxLen`).
   - We traverse the substring using a sliding window approach, updating the character count array (`charCount`) as we go.
   - If the count of distinct characters exceeds `k`, we shrink the window from the left until it's valid again.
   - We update `maxLen` with the maximum length found during traversal.
   - Finally, we return `maxLen` as the length of the longest valid substring.


``` java

public class Solution {

   
    public static int kDistinctChars(int k, String str) {
        
        return kDistinctCharsHelper(k, str, 0, str.length());
    }
    
    
    private static int kDistinctCharsHelper(int k, String str, int start, int end) {
        // Base case: If the substring length is 0, return 0
        if (end - start == 0) {
            return 0;
        }
        
        
        int[] charCount = new int[26]; // Assuming lowercase English letters
        int maxLen = 0; // Initialize the maximum length of the substring
        int uniqueCount = 0; // Initialize the count of unique characters in the substring
        int left = start; // Initialize the left pointer of the sliding window
        
        for (int right = start; right < end; right++) {
            char currentChar = str.charAt(right);
            // If the count of the current character is 0, it's a new distinct character
            if (charCount[currentChar - 'a'] == 0) {
                uniqueCount++;
            }
            charCount[currentChar - 'a']++; // Increment the count of the current character
            
            // Shrink the window if the number of distinct characters exceeds k
            while (uniqueCount > k) {
                char leftChar = str.charAt(left);
                charCount[leftChar - 'a']--; // Decrement the count of the left character
                // If the count becomes 0, decrement the unique count
                if (charCount[leftChar - 'a'] == 0) {
                    uniqueCount--;
                }
                left++; // Move the left pointer to shrink the window
            }
            
            // Update the maximum length of the valid substring if applicable
            maxLen = Math.max(maxLen, right - left + 1);
        }
        
        // Return the maximum length of the valid substring
        return maxLen;
    }
}



```

