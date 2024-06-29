To solve this problem efficiently in O(n) time and O(1) space, we can use two pointers to traverse the strings from the end to the beginning. We simulate the backspace operation by skipping characters that should be removed.

Here's the implementation:

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1;
        int j = t.length() - 1;

        while (i >= 0 || j >= 0) {
            int skipS = 0, skipT = 0;

            // Find position of next possible char in s
            while (i >= 0) {
                if (s.charAt(i) == '#') {
                    skipS++;
                    i--;
                } else if (skipS > 0) {
                    skipS--;
                    i--;
                } else {
                    break;
                }
            }

            // Find position of next possible char in t
            while (j >= 0) {
                if (t.charAt(j) == '#') {
                    skipT++;
                    j--;
                } else if (skipT > 0) {
                    skipT--;
                    j--;
                } else {
                    break;
                }
            }

            // Compare the current character of both strings
            if (i >= 0 && j >= 0 && s.charAt(i) != t.charAt(j)) {
                return false;
            }

            // If comparing a char with an empty position
            if ((i >= 0) != (j >= 0)) {
                return false;
            }

            i--;
            j--;
        }

        return true;
    }
}
```

### Explanation

1. **Initialization**: 
   - We initialize two pointers `i` and `j` to point to the last characters of `s` and `t`, respectively.

2. **Skipping Backspaces in `s`**:
   - We use a `while` loop to traverse `s` from the end. If we encounter a `#`, we increase the `skipS` counter.
   - If we encounter a character and `skipS` is greater than zero, it means this character should be skipped due to a preceding backspace, so we decrement `skipS`.
   - If `skipS` is zero, it means we have found the next valid character in `s` to compare.

3. **Skipping Backspaces in `t`**:
   - Similarly, we traverse `t` from the end, using a `skipT` counter to handle backspaces.

4. **Comparison**:
   - After finding the next valid characters in `s` and `t`, we compare them.
   - If the characters are different, we return `false`.
   - If one string has a character while the other is empty (checked by `(i >= 0) != (j >= 0)`), we return `false`.

5. **Loop**:
   - We continue this process until we have traversed both strings completely.

6. **Return**:
   - If we exit the loop without finding any discrepancies, we return `true`.

This approach ensures we only traverse each string once, making it O(n) time complexity, and we use a constant amount of extra space, achieving O(1) space complexity.
