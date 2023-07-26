# Optimize & Solve Technique
## 1: Look for BUD 
This is perhaps the most useful approach I've found for optimizing problems. "BUD" is a silly acronym for; 
* Bottlenecks 
* Unnecessary work
* Duplicated work
 These are three of the most common things that an algorithm can "waste"time doing. You can walk through 
your brute force looking for these things. When you find one of them, you can then focus on getting rid of it. 
If it's still not optimal, you can repeat this approach on your current best algorithm.


## Unnecessary work
![image](https://github.com/bruhathisp/dsa_java/assets/91585301/aecc4782-63cb-47c8-88b7-781571abca47)

Let's summarize the changes made to the algorithm:

1. **Reducing Unnecessary Checks for d:** By computing `d = pow(a^3 + b^3 - c^3, 1/3)` instead of iterating through all possible d values, we can directly compute the correct integer value for d, eliminating the need for the innermost loop for d. This reduces the runtime complexity from O(N^4) to O(N^3).
``` java 
public class IntegerSolutions {

    public static void main(String[] args) {
        int n = 1000;
        findIntegerSolutions(n);
    }

    public static void findIntegerSolutions(int n) {
        for (int a = 1; a <= n; a++) {
            for (int b = 1; b <= n; b++) {
                for (int c = 1; c <= n; c++) {
                    int sumOfCubes = a * a * a + b * b * b - c * c * c;

                    // Calculate 'd' using the formula: d = cbrt(a^3 + b^3 - c^3)
                    int d = (int) Math.cbrt(sumOfCubes);

                    // Check if 'd' is within the valid range (1 to 1000) and if the equation holds true
                    if (d >= 1 && d <= n && a * a * a + b * b * b == c * c * c + d * d * d) {
                        System.out.println(a + ", " + b + ", " + c + ", " + d);
                    }
                }
            }
        }
    }
}
```


2. **Eliminating Duplicated Work:** Instead of recomputing all (c, d) pairs for each (a, b) pair, the algorithm first creates a map that stores the sum of cubes (`c^3 + d^3`) as keys and corresponding (c, d) pairs as values. Then, when iterating through (a, b) pairs, it looks up the map for the sum (`a^3 + b^3`) and retrieves all matching (c, d) pairs. This approach avoids redundant computations and reduces the runtime complexity from O(N^4) to O(N^2).

Instead of computing all possible (c, d) pairs for each (a, b) pair, we create a map to store the sum of cubes (c^3 + d^3) as keys and the corresponding (c, d) pairs as values.
We iterate through all possible values of c and d, and for each pair, we calculate the sum of cubes (c^3 + d^3).
We then store each (c, d) pair in the map, using the sum of cubes as the key.
After creating the map, we iterate through all possible values of a and b.
For each (a, b) pair, we calculate the sum of cubes (a^3 + b^3) and look it up in the map.
If there are any pairs (c, d) in the map that match the sum, we print the values of a, b, c, and d.
``` java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class IntegerSolutions {

    public static void main(String[] args) {
        int n = 1000;
        findIntegerSolutions(n);
    }

    public static void findIntegerSolutions(int n) {
        Map<Integer, List<Pair>> sumOfCubesMap = new HashMap<>();

        // Step 1: Create a map of sum of cubes (c^3 + d^3) to corresponding (c, d) pairs
        for (int c = 1; c <= n; c++) {
            for (int d = 1; d <= n; d++) {
                int sumOfCubes = c * c * c + d * d * d;
                Pair pair = new Pair(c, d);
                sumOfCubesMap.computeIfAbsent(sumOfCubes, k -> new ArrayList<>()).add(pair);
            }
        }

        // Step 2: Iterate through all (a, b) pairs and find matching (c, d) pairs in the map
        for (int a = 1; a <= n; a++) {
            for (int b = 1; b <= n; b++) {
                int sumOfCubes = a * a * a + b * b * b;
                List<Pair> matchingPairs = sumOfCubesMap.get(sumOfCubes);

                if (matchingPairs != null) {
                    for (Pair pair1 : matchingPairs) {
                        for (Pair pair2 : matchingPairs) {
                            System.out.println(a + ", " + b + ", " + pair1.c + ", " + pair2.d);
                        }
                    }
                }
            }
        }
    }

    static class Pair {
        int c;
        int d;

        public Pair(int c, int d) {
            this.c = c;
            this.d = d;
        }
    }
}
```

The final algorithm's runtime complexity is O(N^2), which is a significant improvement compared to the initial brute-force O(N^4) solution. The elimination of unnecessary iterations and duplicated work optimizes the algorithm for finding positive integer solutions to the equation `a^3 + b^3 == c^3 + d^3`.
