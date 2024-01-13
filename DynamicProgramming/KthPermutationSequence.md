## Solution

### Lessons learnt

1. We divide k by factorial[n - i] to get the index. This tells us how many complete cycles of permutations we can skip to find the right number.
2. Index tells us which number to be added to the permutation

Steps:

1. We have a loop that runs from i = 1 to n, where n is the total number of elements (e.g., if n = 4, we're dealing with numbers from 1 to 4).
2. We divide k by factorial[n - i] to get the index. This tells us how many complete cycles of permutations we can skip to find the right number.
3. We then add the number at position index from our list of available numbers to the result.
4. After adding the number to the result, we remove it from our list of available numbers because we've used it in our permutation.
5. Finally, we update k to reflect the remaining permutations we need to find. We do this by subtracting index * factorial[n - i] from k. This ensures that we consider only the remaining permutations while calculating the next number for our permutation.
6. We repeat this process in each iteration of the loop until we have built the entire k-th permutation.




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

### Testcase
int n = 4;  int k = 9;
```
Initial state of numbers list: [1, 2, 3, 4]
Factorials from 0 to 4: [1, 1, 2, 6, 24]
Iteration 1:
  k: 8
  factorial[3]: 6
 index=  k / factorial[3]: 1
  Selected number: 2
  Updated numbers list: [1, 3, 4]
 index * factorial[n - i]6
  Updated k: 2
Iteration 2:
  k: 2
  factorial[2]: 2
 index=  k / factorial[2]: 1
  Selected number: 3
  Updated numbers list: [1, 4]
 index * factorial[n - i]2
  Updated k: 0
Iteration 3:
  k: 0
  factorial[1]: 1
 index=  k / factorial[1]: 0
  Selected number: 1
  Updated numbers list: [4]
 index * factorial[n - i]0
  Updated k: 0
Iteration 4:
  k: 0
  factorial[0]: 1
 index=  k / factorial[0]: 0
  Selected number: 4
  Updated numbers list: []
 index * factorial[n - i]0
  Updated k: 0
The 9-th permutation of numbers from 1 to 4 is: 2314

```


``` java
    import java.util.ArrayList;
import java.util.List;

public class MyClass {
    public static String getKthPermutation(int n, int k) {
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

        System.out.println("Initial state of numbers list: " + numbers); // Print initial state of numbers list
        System.out.println("Factorials from 0 to " + n + ": " + java.util.Arrays.toString(factorial)); // Print all precomputed factorials

        for (int i = 1; i <= n; i++) {
            int index = k / factorial[n - i];
            System.out.println("Iteration " + i + ":");
            System.out.println("  k: " + k);
            System.out.println("  factorial[" + (n - i) + "]: " + factorial[n - i]);
            System.out.println(" index=  k / factorial[" + (n - i) + "]: " + index); // Print how many complete cycles are skipped
            result.append(numbers.get(index));
            System.out.println("  Selected number: " + numbers.get(index)); // Print selected number
            numbers.remove(index);
            System.out.println("  Updated numbers list: " + numbers +"\n index * factorial[n - i]"+(index * factorial[n - i])); // Print updated numbers list
            k -= index * factorial[n - i];
            System.out.println("  Updated k: " + k);
        }

        return result.toString();
    }

    private static void computeFactorials(int[] factorial) {
        factorial[0] = 1;
        for (int i = 1; i < factorial.length; i++) {
            factorial[i] = factorial[i - 1] * i;
        }
    }

    public static void main(String[] args) {
        int n = 4;
        int k = 9;
        String permutation = getKthPermutation(n, k);
        System.out.println("The " + k + "-th permutation of numbers from 1 to " + n + " is: " + permutation);
    }
}

```
