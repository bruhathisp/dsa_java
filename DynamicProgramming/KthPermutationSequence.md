## Solution


``` java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public String getKthPermutation(int n, int k) {
        List<Integer> numbers = new ArrayList<>();
        int[] factorial = new int[n + 1];
        StringBuilder result = new StringBuilder();

        // Create a list of numbers from 1 to n
        for (int i = 1; i <= n; i++) {
            numbers.add(i);
        }

        // Precompute factorials for efficiency
        computeFactorials(factorial);

        // Adjust k to 0-based index
        k--;

        for (int i = 1; i <= n; i++) {
            int index = k / factorial[n - i];
            result.append(numbers.get(index));
            numbers.remove(index);
            k -= index * factorial[n - i];
        }

        return result.toString();
    }

    private void computeFactorials(int[] factorial) {
        factorial[0] = 1;
        for (int i = 1; i < factorial.length; i++) {
            factorial[i] = factorial[i - 1] * i;
        }
    }

    
}

```
