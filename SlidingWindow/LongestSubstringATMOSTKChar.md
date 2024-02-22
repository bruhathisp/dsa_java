## Solution

[Question](https://www.codingninjas.com/studio/problems/longest-substring-with-at-most-k-distinct-characters_2221410?leftPanelTabValue=PROBLEM)



In this code:

- We maintain a sliding window `[left, right]` to iterate over the string.
- We use an array `charCount` to keep track of the count of each character in the current window.
- The variable `distinctCount` keeps track of the number of distinct characters in the current window.
- We increment `distinctCount` only when encountering a new character.
- If `distinctCount` exceeds `K`, we shrink the window from the left until `distinctCount` becomes less than or equal to `K`.
- We update `maxLength` whenever we encounter a window with at most `K` distinct characters.
- Finally, we return `maxLength` as the result.


``` java

public class Solution {

	public static int kDistinctChars(int k, String str) {
		
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

