# diamond pattern

``` java

public class DiamondPattern {

    // Method to print the diamond pattern
    static void printDiamond(int n) {
        // Print the upper part of the diamond
        for (int i = 1; i <= n; i++) {
            // Print leading spaces
            for (int j = i; j < n; j++) {
                System.out.print(" ");
            }
            // Print asterisks
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        
        // Print the lower part of the diamond
        for (int i = n - 1; i >= 1; i--) {
            // Print leading spaces
            for (int j = n; j > i; j--) {
                System.out.print(" ");
            }
            // Print asterisks
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }

    // Main method
    public static void main(String[] args) {
        int n = 5;
        printDiamond(n); // Print the diamond pattern
    }
}



```
``` 
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *

```


[Sort by freq ](https://www.geeksforgeeks.org/problems/sorting-elements-of-an-array-by-frequency/0)
``` java
/*package whatever //do not write package name here */

import java.util.*;
import java.lang.*;
import java.io.*;

class GFG {
	public static void main (String[] args) {
		//code

        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();
        while (T-- > 0) {
            int N = sc.nextInt();
            int[] arr = new int[N];
            for (int i = 0; i < N; i++) {
                arr[i] = sc.nextInt();
            }
            int[] result = sortByFrequency(arr);
            for (int num : result) {
                System.out.print(num + " ");
            }
            System.out.println();
        }
        sc.close();
    }

    public static int[] sortByFrequency(int[] arr) {
        // Step 2: Count Frequencies
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : arr) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Step 3: Create a list of the array elements
        List<int[]> elementList = new ArrayList<>();
        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            elementList.add(new int[]{entry.getKey(), entry.getValue()});
        }

        // Sort by frequency (descending) and by element value (ascending)
        Collections.sort(elementList, (a, b) -> {
            if (b[1] == a[1]) {
                return a[0] - b[0]; // If frequencies are the same, sort by value (ascending)
            }
            return b[1] - a[1]; // Otherwise, sort by frequency (descending)
        });

        // Step 4: Construct the result array
        int[] result = new int[arr.length];
        int index = 0;
        for (int[] element : elementList) {
            int value = element[0];
            int frequency = element[1];
            for (int i = 0; i < frequency; i++) {
                result[index++] = value;
            }
        }

        return result;
    }
}
```
[check-for-subsequence](https://www.geeksforgeeks.org/problems/check-for-subsequence4930/1)
``` java
public class SubsequenceCheck {

    public static boolean isSubSequence(String A, String B) {
        int i = 0; // Pointer for A
        int j = 0; // Pointer for B

        // Traverse B
        while (j < B.length()) {
            if (i < A.length() && A.charAt(i) == B.charAt(j)) {
                i++;
            }
            j++;
        }

        // If we have traversed all characters of A
        return i == A.length();
    }

}
```


