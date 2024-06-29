[Question](https://leetcode.com/problems/squares-of-a-sorted-array/)

# Solution 

Lesson learnt- 
1. ALWAYS create a new array in case of an operation+sorting
2. Use two pointers when you have to sort with O(n) complexity
3. When to initiate an array of length use, `int[] result = new int[nums.length];`


Sorting: If your primary goal is to sort a collection of elements, especially if you're dealing with a large dataset, `merge` sort or other efficient sorting algorithms (like quicksort) are appropriate.
Pointers: Searching or Manipulating: If you need to efficiently search for elements or perform operations on elements based on some condition, the `two-pointer` technique might be more suitable.
It can be more efficient than some sorting algorithms for specific tasks, especially when the array is already partially sorted.
It's particularly useful in situations where you need to iterate through the array and compare or manipulate elements efficiently.


## Approach

Input [-5,-4,-3,-2-1]

Output  [1, 4, 9, 25]


The issue in my code arises when I directly modified the `nums` array during the sorting process. This leads to incorrect results because the values in the array are changing, and the changes affect the subsequent comparisons.

Let's break down the problematic part of my previous code:

Copy and run this code 

```java
import java.io.*;
import java.util.*;

class Main {
    public static int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int index = right;

        // Using the two-pointer technique to sort
        while (left <= right) {
            // Swap elements if they are out of order
            int square1= nums[left] * nums[left];
            int square2 = nums[right] * nums[right];
           if (square1 >square2) {
               int temp= nums[right];
                nums[right] = square1;
                nums[left]= temp;
                right--;
            } else {
                
                nums[right]= square2;
                right--;
            }
            
            System.out.println("Index: " + index + ", Left: " + left + ", Right: " + right + ", Square1: " + square1 + ", Square2: " + square2 + ", nums: " + Arrays.toString(nums));
        }

        

        return nums;
    }
    public static void main(String[] args) {
        
        int[] nums = {-5, -3, -2, -1};
        int[] result = sortedSquares(nums);
        for (int i = 0; i < result.length; i++) {
            System.out.print(result[i] + " ");
        }
    }
    
    
}

```
## Result for code 1 

Index: 3, Left: 0, Right: 2, Square1: 25, Square2: 1, nums: [-1, -3, -2, 25]


Index: 3, Left: 0, Right: 1, Square1: 1, Square2: 4, nums: [-1, -3, 4, 25]


Index: 3, Left: 0, Right: 0, Square1: 1, Square2: 9, nums: [-1, 9, 4, 25]


Index: 3, Left: 0, Right: -1, Square1: 1, Square2: 1, nums: [1, 9, 4, 25]



In the `if (square1 > square2)` block, you're modifying the `nums` array directly by swapping values. However, during this process, you're changing the value at index `left` to the square of `nums[right]`. This interferes with the subsequent iterations and causes unexpected results.

To resolve this issue, you should use a separate array, like your `result` array, to store the sorted square values. This ensures that the original array (`nums`) remains unchanged during the sorting process, and the results are correct.

## The corrected code would look like this:

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int index = right;
        int[] result = new int[nums.length];

        // Using the two-pointer technique to sort
        while (left <= right) {
            int square1 = nums[left] * nums[left];
            int square2 = nums[right] * nums[right];

            if (square1 > square2) {
                result[index--] = square1;
                left++;
            } else {
                result[index--] = square2;
                right--;
            }
        }

        return result;
    }
}
```
## Result for code 2 

Index: 2, Left: 1, Right: 3, Square1: 25, Square2: 1, result: [0, 0, 0, 25]

Index: 1, Left: 2, Right: 3, Square1: 9, Square2: 1, result: [0, 0, 9, 25]

Index: 0, Left: 3, Right: 3, Square1: 4, Square2: 1, result: [0, 4, 9, 25]

Index: -1, Left: 3, Right: 2, Square1: 1, Square2: 1, result: [1, 4, 9, 25]



Now, the `result` array is used to store the sorted square values, and the `nums` array remains unchanged during the sorting process. This ensures that the final result is correct.
