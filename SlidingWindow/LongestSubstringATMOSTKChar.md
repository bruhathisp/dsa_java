## Solution

[Question](https://www.codingninjas.com/studio/problems/longest-substring-with-at-most-k-distinct-characters_2221410?leftPanelTabValue=PROBLEM)

You are given ‘str’ = ‘abbbbbbc’ and ‘K’ = 2, then the substrings can be formed are [‘abbbbbb’, ‘bbbbbbc’]. Hence the answer is 7.


   - Our goal is to find the length of the longest substring in `str` that contains at most `k` different characters.
   - If the number of distinct characters in the substring exceeds `k`, we'll shrink  window from the left until it's valid again.

2. **Code Explanation**:
   - We initialize variables to keep track of the count of unique characters (`uniqueCount`), the left pointer of the window (`left`), and the maximum length of the valid substring (`maxLen`).
   - We traverse the substring using a sliding window approach, updating the character count array (`charCount`) as we go.
   - If the count of distinct characters exceeds `k`, we shrink the window from the left until it's valid again.
   - We update `maxLen` with the maximum length found during traversal.
   - Finally, we return `maxLen` as the length of the longest valid substring.

``` java

public class Solution {

	public static int kDistinctChars(int k, String str) {
		// Write your code here

		

		int[] charCount = new int[26]; // Assuming lowercase alphabets
        int distinctCount = 0;
        int maxLength = 0;
        int left = 0;

        // Iterate over the string using a sliding window
        for (int right = 0; right < str.length(); right++) {
            // Process the character at the right pointer
            char currentChar = str.charAt(right);
            if (charCount[currentChar - 'a'] == 0) {
                distinctCount++; // Increment distinct character count
            }
            charCount[currentChar - 'a']++; // Update character count

            // Shrink the window if the number of distinct characters exceeds K
            while (distinctCount > k) {
                char leftChar = str.charAt(left);
                charCount[leftChar - 'a']--; // Decrement count of left character
                if (charCount[leftChar - 'a'] == 0) {
                    distinctCount--; // Decrement distinct character count
                }
                left++; // Move the left pointer to shrink the window
            }

            // Update the maximum length of the substring
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
	}

}

```
