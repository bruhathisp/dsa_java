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

# The Sieve of Eratosthenes 
``` java
public class SieveOfEratosthenes {

    // Function to find all prime numbers up to a given limit
    static void sieveOfEratosthenes(int n) {
        // Create a boolean array "prime[0..n]" and initialize all entries it as true.
        // A value in prime[i] will finally be false if i is Not a prime, else true.
        boolean[] prime = new boolean[n + 1];
        for (int i = 0; i <= n; i++) {
            prime[i] = true;
        }

        for (int p = 2; p * p <= n; p++) {
            // If prime[p] is not changed, then it is a prime
            if (prime[p]) {
                // Update all multiples of p to not prime
                for (int i = p * p; i <= n; i += p) {
                    prime[i] = false;
                }
            }
        }

        // Print all prime numbers
        for (int p = 2; p <= n; p++) {
            if (prime[p]) {
                System.out.print(p + " ");
            }
        }
    }

    // Main method
    public static void main(String[] args) {
        int n = 30; // Change this value to find primes up to n
        System.out.println("Prime numbers up to " + n + ":");
        sieveOfEratosthenes(n);
    }
}
```

## find the prime factor
``` java
import java.util.ArrayList;
import java.util.List;

public class PrimeFactors {

    // Function to find and return the prime factors of a given number
    public static List<Integer> findPrimeFactors(int n) {
        List<Integer> primeFactors = new ArrayList<>();

        // Divide n by 2 until it is odd
        while (n % 2 == 0) {
            primeFactors.add(2);
            n /= 2;
        }

        // n must be odd at this point
        // So we start from 3 and check only odd numbers
        for (int i = 3; i <= Math.sqrt(n); i += 2) {
            // While i divides n, add i and divide n
            while (n % i == 0) {
                primeFactors.add(i);
                n /= i;
            }
        }

        // This condition is to handle the case when n is a prime number
        // greater than 2
        if (n > 2) {
            primeFactors.add(n);
        }

        return primeFactors;
    }

    // Main method for testing
    public static void main(String[] args) {
        int n = 315; // Example number
        List<Integer> primeFactors = findPrimeFactors(n);
        System.out.println("Prime factors of " + n + " are: " + primeFactors);
    }
}

```
[Question](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)

``` java

 public class SingleElementInSortedArray {

    public static int singleNonDuplicate(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            // Ensure the mid is even for comparison
            if (mid % 2 == 1) {
                mid--;
            }

            // Check if the pair is valid
            if (nums[mid] == nums[mid + 1]) {
                left = mid + 2; // Move to the right half
            } else {
                right = mid; // Move to the left half
            }
        }

        return nums[left];
    }
```
Yes, you are correct! The expression result |= (1 << i) effectively adds 2^i to the result if the i-th bit in bitCount is not a multiple of 3. Let's break down how this works:  https://leetcode.com/problems/single-number-ii/description/
``` java

```
