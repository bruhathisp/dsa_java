## Solution


Sorted the array before applying the wave transformation.


``` java
import java.util.Arrays;

public class Solution {
    public static int[] wave(int[] A) {
        int n = A.length;
        if (n == 0 || n == 1) return A;

        Arrays.sort(A);

        for (int i = 0; i < n - 1; i += 2) {
            swap(A, i, i + 1);
        }

        return A;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] A = {5, 1, 3, 2, 4};

        System.out.println(Arrays.toString(wave(A))); // Output: [2, 1, 4, 3, 5]
    }
}

```



## Optimised O(n) Time Complexity. not possible

