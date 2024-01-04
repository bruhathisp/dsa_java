## Soltion

Lesson Learnt

1. Without this code we cannot write the code within time limit 

How do yo find whether the character at the current position (`chars[i]`) has been used before in the permutation process. Let me break it down:

- `chars[i] - 'a'`: This expression converts the character to its corresponding index in the boolean array. For example, if `chars[i]` is 'a', then `chars[i] - 'a'` is 0; if it's 'b', then it's 1, and so on. This conversion is commonly used to map characters to indices, especially when dealing with lowercase English letters.

- `used[chars[i] - 'a']`: This checks the boolean array to see if the character has been used before. If `used[chars[i] - 'a']` is `false`, it means the character hasn't been used, and we can proceed to include it in the permutation.

- `used[chars[i] - 'a'] = true;`: Once the character is included in the permutation, we mark it as used to prevent its repetition in subsequent positions of the permutation.

This mechanism helps avoid generating duplicate permutations. The `used` array ensures that each character is considered only once at a particular position in the permutation process. If the character has been used before, the algorithm skips it, preventing the generation of duplicate permutations.


2. If you choose an element at a certain position but later realize it doesn't work, you need to undo that choice and explore other possibilities. Swapping allows you to backtrack efficiently.



``` java
  import java.util.ArrayList;
import java.util.List;

class Solution {
    List<String> getAllPermutations(String s) {
        List<String> result = new ArrayList<>();
        char[] chars = s.toCharArray();
        backtrack(chars, 0, result);
        return result;
    }

    private void backtrack(char[] chars, int start, List<String> result) {
        if (start == chars.length - 1) {
            result.add(new String(chars));
            return;
        }

        boolean[] used = new boolean[26]; // Assuming only lowercase English characters
        for (int i = start; i < chars.length; i++) {
            if (!used[chars[i] - 'a']) {
                used[chars[i] - 'a'] = true;   //update the used array
                swap(chars, start, i);  // you need to undo that choice and explore other possibilities. Swapping allows you to backtrack efficiently.
                backtrack(chars, start + 1, result);
                swap(chars, start, i); // Backtrack to the original state
            }
        }
    }

    private void swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
    }
}

```
