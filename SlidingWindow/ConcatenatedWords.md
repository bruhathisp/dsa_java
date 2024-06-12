[Question](https://leetcode.com/problems/concatenated-words/description/)


To solve the problem of finding all concatenated words in a given list of words, we can use dynamic programming combined with a set for efficient lookups. The idea is to check each word to see if it can be formed by concatenating other words from the list.

### Approach

1. **Use a Set for Fast Lookups**:
   - Store all words in a set for O(1) time complexity for lookups.

2. **Dynamic Programming**:
   - For each word, use a dynamic programming array `dp` where `dp[i]` indicates whether the substring `word[0:i]` can be formed by concatenating other words from the list.
   - Initialize `dp[0]` to `true` because an empty substring can always be formed.
   - For each position `i` in the word, check all possible substrings `word[j:i]` where `j` ranges from `0` to `i-1`. If `dp[j]` is `true` and the substring `word[j:i]` exists in the set, then set `dp[i]` to `true`.

3. **Check Concatenated Words**:
   - For each word, skip the word itself when checking if it can be formed by other words.

### Implementation

Here's the Java implementation:

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        Set<String> wordSet = new HashSet<>();
        List<String> result = new ArrayList<>();

        for (String word : words) {
            wordSet.add(word);
        }

        for (String word : words) {
            if (canForm(word, wordSet)) {
                result.add(word);
            }
        }

        return result;
    }

    private boolean canForm(String word, Set<String> wordSet) {
        if (word.isEmpty()) return false;
        int n = word.length();
        boolean[] dp = new boolean[n + 1];
        dp[0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordSet.contains(word.substring(j, i)) && !word.substring(j, i).equals(word)) {
                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[n];
    }
}
```

### Explanation

1. **Initialization**:
   - Store all words in a `HashSet` called `wordSet` for O(1) lookup time.

2. **Check Each Word**:
   - For each word in the list, use the helper function `canForm` to check if it can be formed by concatenating other words from `wordSet`.

3. **Helper Function `canForm`**:
   - Initialize a `dp` array where `dp[i]` is `true` if the substring `word[0:i]` can be formed by concatenating other words.
   - Iterate through each position `i` in the word.
   - For each `i`, check all possible substrings `word[j:i]`.
   - If `dp[j]` is `true` and `word[j:i]` exists in `wordSet` (and it is not the same as `word` itself), set `dp[i]` to `true`.

4. **Return Results**:
   - Collect all words that can be formed by concatenation and return them.

### Examples:

**Example 1**:
- **Input**: `words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]`
- **Output**: `["catsdogcats","dogcatsdog","ratcatdogcat"]`
- **Explanation**:
  - "catsdogcats" is formed by "cats", "dog", and "cats".
  - "dogcatsdog" is formed by "dog", "cats", and "dog".
  - "ratcatdogcat" is formed by "rat", "cat", "dog", and "cat".

**Example 2**:
- **Input**: `words = ["cat","dog","catdog"]`
- **Output**: `["catdog"]`
- **Explanation**:
  - "catdog" is formed by "cat" and "dog".

### Complexity:
- **Time Complexity**: O(n \* m^2), where n is the number of words and m is the average length of the words. This is because for each word, we are iterating over all possible substrings.
- **Space Complexity**: O(n \* m), for the `dp` array and the `wordSet`.

This solution efficiently finds all concatenated words in the list using dynamic programming and set operations.
