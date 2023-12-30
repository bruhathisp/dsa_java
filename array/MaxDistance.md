## Solution


Step 1: Initialize variables
    Set n as array length
    Create an array `maxRight` of size n to store the maximum element on the right.
    

Step 2: Calculate maxRight array
    Set maxRight[n-1] to A[n-1].
    Loop from i = n-2 to 0 and Calculate the maximum element on the right for each index
        `maxRight[i] = Math.max(maxRight[i + 1], A[i]);`

Step 3: Find maximum gap
    Initialize two pointers, i and j, to 0. `maxDistance = Integer.MIN_VALUE;`
    Loop while i < n and j < n:
        ``` java 
        If A[i] <= maxRight[j]:
            Update maxDistance to  max( of maxDistance and j - i)
            Increment j.
        Else:
            Increment i.
    ```
Step 4: Return maxDistance as the result.


``` java
public class Solution {
    // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
    public int maximumGap(final int[] A) {
    int n = A.length;

        // Arrays to store the maximum element on the right
        int[] maxRight = new int[n];

        
        // Calculate the maximum element on the right for each index
        maxRight[n - 1] = A[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            maxRight[i] = Math.max(maxRight[i + 1], A[i]);
        }

        // Iterate through the array to find the maximum gap
        int i = 0, j = 0, maxDistance = Integer.MIN_VALUE;
        while (i < n && j < n) {
            
            if (A[i] <= maxRight[j]) {
                maxDistance = Math.max(maxDistance, j - i);
                
                j++;
            } else {
                i++;
            }

        }

        return maxDistance;
    }

}

```

Output 

Input:  A = [3, 5, 4, 2]

Output1:  2



### Iteration: 1

Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 0, A[0]: 3
Current j: 0, A[0]: 3

maxRight[0]: 5

maxDistance updated: 0

New i: 0, New j: 1

### Iteration: 2
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 0, A[0]: 3
Current j: 1, A[1]: 5

maxRight[1]: 5

maxDistance updated: 1
New i: 0, New j: 2

### Iteration: 3
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 0, A[0]: 3
Current j: 2, A[2]: 4

maxRight[2]: 4

maxDistance updated: 2
New i: 0, New j: 3

### Iteration: 4
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 0, A[0]: 3
Current j: 3, A[3]: 2

maxRight[3]: 2

New i: 1, New j: 3

### Iteration: 5
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 1, A[1]: 5
Current j: 3, A[3]: 2

maxRight[3]: 2

New i: 2, New j: 3

### Iteration: 6
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 2, A[2]: 4
Current j: 3, A[3]: 2

maxRight[3]: 2

New i: 3, New j: 3

### Iteration: 7
Array A: [3, 5, 4, 2]
maxRight: [5, 5, 4, 2]

Current i: 3, A[3]: 2
Current j: 3, A[3]: 2

maxRight[3]: 2
New i: 3, New j: 4

maxDistance updated: 2


2





## Code to generate the above 

``` java
    import java.util.Arrays;

public class MyClass {
    // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
    public static int maximumGap(final int[] A) {
        int n = A.length;

        // Arrays to store the maximum element on the right
                int[] maxRight = new int[n];

        

        // Calculate the maximum element on the right for each index
        maxRight[n - 1] = A[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            maxRight[i] = Math.max(maxRight[i + 1], A[i]);
        }

        // Iterate through the array to find the maximum gap
        int i = 0, j = 0, maxDistance = Integer.MIN_VALUE;
        while (i < n && j < n) {
            System.out.println("Iteration: " + (i + j + 1));
            System.out.println("Array A: " + Arrays.toString(A));
            System.out.println("minLeft: " + Arrays.toString(minLeft));
            System.out.println("maxRight: " + Arrays.toString(maxRight));
            System.out.println("Current i: " + i + ", A[" + i + "]: " + A[i]);
            System.out.println("Current j: " + j + ", A[" + j + "]: " + A[j]);
            System.out.println("minLeft[" + i + "]: " + minLeft[i]);
            System.out.println("maxRight[" + j + "]: " + maxRight[j]);

            if (A[i] <= maxRight[j]) {
                maxDistance = Math.max(maxDistance, j - i);
                System.out.println("maxDistance updated: " + maxDistance);
                j++;
            } else {
                i++;
            }

            System.out.println("New i: " + i + ", New j: " + j);
            System.out.println();
        }

        return maxDistance;
    }

    public static void main(String[] args) {
        int[] A = {3, 5, 4, 2};
        System.out.println(maximumGap(A)); // Output: 2
    }
}

```
