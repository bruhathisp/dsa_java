## Solution

The main difference between Combination Sum I and Combination Sum II is how duplicates are handled in the input array. Let's break down the differences:

**Combination Sum I:**
- **Goal:** Find all unique combinations of integers from the array whose sum is equal to the target value.
- **Handling Duplicates:** The input array can contain duplicate elements, but the result should not have duplicate combinations.
- **Example:**
  - Input: `[2, 3, 6, 7]`, target: `7`
  - Combinations: `[[2, 2, 3], [7]]`

**Combination Sum II:**
- **Goal:** Find all unique combinations of integers from the array whose sum is equal to the target value.
- **Handling Duplicates:** The input array can contain duplicate elements, and the result may have duplicate combinations if the input array has duplicate elements.
- **Example:**
  - Input: `[10, 1, 2, 7, 6, 1, 5]`, target: `8`
  - Combinations: `[[1, 1, 6], [1, 2, 5], [1, 7], [2, 6]]`
  - Note: `[1, 1, 6]` and `[1, 2, 5]` are distinct combinations even though they have duplicate elements.

In summary, Combination Sum I ensures that the result has unique combinations, while Combination Sum II allows duplicate combinations in the result if the input array contains duplicate elements.


The `Arrays.sort(A)` is added to sort the array, and a check `if (i > start && A[i] == A[i - 1])` is added inside the loop to skip duplicates and avoid generating duplicate combinations.

Here is the code for Combination Sum II in the desired format:

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    List<List<Integer>> combinationSum(int[] A, int val) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(A); // Sort the array to handle duplicates properly
        backtrack(A, val, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] A, int target, int start, List<Integer> currentSum, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(currentSum));
            return;
        }

        for (int i = start; i < A.length; i++) {
            // Skip duplicates to avoid duplicate combinations
            if (i > start && A[i] == A[i - 1]) {
                continue;
            }

            if (target - A[i] >= 0) {
                currentSum.add(A[i]);
                backtrack(A, target - A[i], i + 1, currentSum, result); // Use i + 1 to avoid reusing the same element
                currentSum.remove(currentSum.size() - 1);
            }
        }
    }
}
```

This code is similar to the Combination Sum I code with a slight modification to handle duplicates. 
