## Solution

The primary difference between the two `wordBreak` 1 and 2 implementations lies in their approaches and how they handle the results:

**Word Break 1:**
- Uses a boolean array `dp` to keep track of whether substrings of the input string can be formed using words from the list.
- The goal is to determine whether the entire input string can be formed using words from the list.

**Word Break 2:**
- Uses an array of `ArrayList<String>` (`dp`) to store the words that can be used to form substrings at each position in the input string.
- Uses a depth-first search (DFS) approach to generate all possible combinations of words that can be used to form the input string.
- The goal is not only to determine if the input string can be formed but also to find all possible combinations of words that form the string.

In simple terms, Word Break 1 is focused on checking whether a string can be formed, while Word Break 2 goes further and generates all possible ways of forming the string using the provided list of words.

The first implementation (Word Break 1) is typically more efficient for the task of checking whether a string can be formed, while the second implementation (Word Break 2) is more complex and is designed to provide additional information about the possible combinations of words forming the string.


## Code for word break 1 

``` java
   class Solution {
    boolean wordBreak(String s, String[] w) {
        ArrayList<String>[] dp = new ArrayList[s.length() + 1];
        dp[0] = new ArrayList<>();

        for (int i = 0; i <= s.length(); i++) {
            if (dp[i] == null)
                continue;

            for (String word : w) {
                int length = word.length();
                int end = i + length;

                if (end > s.length())
                    continue;

                if (s.substring(i, end).equals(word)) {
                    if (dp[end] == null)
                        dp[end] = new ArrayList<>();
                    dp[end].add(word);
                }
            }
        }

        if (dp[s.length()] == null)
            return false;

        return true;
    }
}

```
