# Day 1


### 1. **Remove Duplicates from Sorted Array**
   - **LeetCode Problem**: [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
   - **Problem Breakdown**: You are given a sorted array `nums`. You need to remove duplicates in-place such that each unique element appears only once, and return the new length.
   - **Time Complexity**: O(n) (where n is the length of the array)
   - **Space Complexity**: O(1) (in-place modification)
   - **Java Solution**:
     ```java
     class Solution {
         public int removeDuplicates(int[] nums) {
             if (nums.length == 0) return 0;
             int i = 0;
             for (int j = 1; j < nums.length; j++) {
                 if (nums[j] != nums[i]) {
                     i++;
                     nums[i] = nums[j];
                 }
             }
             return i + 1;
         }
     }
     ```
   - **Explanation**: This solution uses two pointers, `i` to track the position of the last unique element and `j` to explore the array. If a new unique element is found, it is placed at the `i+1` position.

### 2. **Find the Duplicate Number** (Binary Search Approach)
   - **LeetCode Problem**: [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
   - **Approach**: Binary Search on Counts
     This solution leverages the fact that if you count how many numbers are less than or equal to a certain value, it gives clues about where the duplicate lies.
   
   - **Time Complexity**: O(n log n)
   - **Space Complexity**: O(1)
   
   - **Java Solution**:
     ```java
     class Solution {
         public int findDuplicate(int[] nums) {
             int left = 1, right = nums.length - 1;
             
             while (left < right) {
                 int mid = left + (right - left) / 2;
                 int count = 0;
                 
                 // Count numbers less than or equal to mid
                 for (int num : nums) {
                     if (num <= mid) {
                         count++;
                     }
                 }
                 
                 // If count is greater than mid, the duplicate must be in the lower half
                 if (count > mid) {
                     right = mid;
                 } else {
                     left = mid + 1;
                 }
             }
             
             return left;
         }
     }
     ```
   - **Explanation**:
     - The algorithm applies **binary search** on the range of numbers from `1` to `n` (not the array indices). 
     - For each midpoint `mid`, it counts how many numbers in the array are less than or equal to `mid`.
     - If there are more numbers than expected (i.e., `count > mid`), this indicates the duplicate lies in the range `[left, mid]`.
     - Otherwise, the duplicate is in the range `[mid + 1, right]`.
     - The search continues until the range narrows to a single number, which is the duplicate.

This binary search solution is efficient because it doesn’t rely on modifying the array or using additional space beyond simple variables.


### 3. **Maximum Subarray**
   - **LeetCode Problem**: [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
   - **Problem Breakdown**: Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public int maxSubArray(int[] nums) {
             int maxSum = nums[0];
             int currentSum = nums[0];
             
             for (int i = 1; i < nums.length; i++) {
                 currentSum = Math.max(nums[i], currentSum + nums[i]);
                 maxSum = Math.max(maxSum, currentSum);
             }
             
             return maxSum;
         }
     }
     ```
   - **Explanation**: This uses Kadane’s Algorithm, which keeps track of the maximum subarray sum ending at each index. At each step, it either starts a new subarray or adds to the existing one.

### 4. **Contains Duplicate**
   - **LeetCode Problem**: [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
   - **Problem Breakdown**: Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(n)
   - **Java Solution**:
     ```java
     import java.util.HashSet;

     class Solution {
         public boolean containsDuplicate(int[] nums) {
             HashSet<Integer> set = new HashSet<>();
             for (int num : nums) {
                 if (!set.add(num)) {
                     return true;
                 }
             }
             return false;
         }
     }
     ```
   - **Explanation**: This solution uses a hash set to track unique elements as it iterates through the array. If a duplicate is found, it returns `true` immediately.


# Day 2


### 1. **Set Matrix Zeroes**
   - **LeetCode Problem**: [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
   - **Problem Breakdown**: Given an `m x n` matrix, if an element is 0, set its entire row and column to 0. This should be done **in-place**.
   - **Time Complexity**: O(m * n) (where m is the number of rows and n is the number of columns)
   - **Space Complexity**: O(1) (constant space excluding input matrix)
   - **Java Solution**:
     ```java
     class Solution {
         public void setZeroes(int[][] matrix) {
             boolean firstRowZero = false, firstColZero = false;
             int rows = matrix.length, cols = matrix[0].length;
             
             // Check if the first row and first column need to be zeroed
             for (int i = 0; i < rows; i++) {
                 if (matrix[i][0] == 0) firstColZero = true;
             }
             for (int j = 0; j < cols; j++) {
                 if (matrix[0][j] == 0) firstRowZero = true;
             }
             
             // Use the first row and column as markers
             for (int i = 1; i < rows; i++) {
                 for (int j = 1; j < cols; j++) {
                     if (matrix[i][j] == 0) {
                         matrix[i][0] = 0;
                         matrix[0][j] = 0;
                     }
                 }
             }
             
             // Zero out cells based on markers
             for (int i = 1; i < rows; i++) {
                 for (int j = 1; j < cols; j++) {
                     if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                         matrix[i][j] = 0;
                     }
                 }
             }
             
             // Handle the first row and column separately
             if (firstColZero) {
                 for (int i = 0; i < rows; i++) {
                     matrix[i][0] = 0;
                 }
             }
             if (firstRowZero) {
                 for (int j = 0; j < cols; j++) {
                     matrix[0][j] = 0;
                 }
             }
         }
     }
     ```
   - **Explanation**: This solution uses the first row and column as markers for zeroing the entire row or column. It checks and handles the first row and column separately to avoid overwriting their information prematurely.

### 2. **Spiral Matrix**
   - **LeetCode Problem**: [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
   - **Problem Breakdown**: Given an `m x n` matrix, return all elements of the matrix in spiral order.
   - **Time Complexity**: O(m * n)
   - **Space Complexity**: O(1) (not counting the output array)
   - **Java Solution**:
     ```java
     import java.util.ArrayList;
     import java.util.List;

     class Solution {
         public List<Integer> spiralOrder(int[][] matrix) {
             List<Integer> result = new ArrayList<>();
             if (matrix.length == 0) return result;

             int top = 0, bottom = matrix.length - 1;
             int left = 0, right = matrix[0].length - 1;

             while (top <= bottom && left <= right) {
                 // Traverse from left to right across the top row
                 for (int j = left; j <= right; j++) {
                     result.add(matrix[top][j]);
                 }
                 top++;

                 // Traverse from top to bottom down the right column
                 for (int i = top; i <= bottom; i++) {
                     result.add(matrix[i][right]);
                 }
                 right--;

                 if (top <= bottom) {
                     // Traverse from right to left across the bottom row
                     for (int j = right; j >= left; j--) {
                         result.add(matrix[bottom][j]);
                     }
                     bottom--;
                 }

                 if (left <= right) {
                     // Traverse from bottom to top up the left column
                     for (int i = bottom; i >= top; i--) {
                         result.add(matrix[i][left]);
                     }
                     left++;
                 }
             }

             return result;
         }
     }
     ```
   - **Explanation**: The matrix is traversed layer by layer in a spiral order, updating the boundaries (`top`, `bottom`, `left`, `right`) as each layer is processed.

---

### 3. **Rotate Image**
   - **LeetCode Problem**: [Rotate Image](https://leetcode.com/problems/rotate-image/)
   - **Problem Breakdown**: Given an `n x n` 2D matrix representing an image, rotate the image by 90 degrees (clockwise) **in-place**.
   - **Time Complexity**: O(n^2)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public void rotate(int[][] matrix) {
             int n = matrix.length;
             
             // Transpose the matrix (swap elements across the diagonal)
             for (int i = 0; i < n; i++) {
                 for (int j = i; j < n; j++) {
                     int temp = matrix[i][j];
                     matrix[i][j] = matrix[j][i];
                     matrix[j][i] = temp;
                 }
             }
             
             // Reverse each row
             for (int i = 0; i < n; i++) {
                 for (int j = 0; j < n / 2; j++) {
                     int temp = matrix[i][j];
                     matrix[i][j] = matrix[i][n - j - 1];
                     matrix[i][n - j - 1] = temp;
                 }
             }
         }
     }
     ```
   - **Explanation**: The matrix is rotated by first transposing it (swapping across the diagonal) and then reversing each row to achieve a 90-degree clockwise rotation.

### 4. **Search a 2D Matrix**
   - **LeetCode Problem**: [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
   - **Problem Breakdown**: Given an `m x n` matrix where each row is sorted, and the first integer of each row is greater than the last integer of the previous row, write an efficient algorithm to search for a target value.
   - **Time Complexity**: O(log(m * n)) (binary search on a 1D view of the matrix)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public boolean searchMatrix(int[][] matrix, int target) {
             if (matrix.length == 0 || matrix[0].length == 0) return false;
             
             int rows = matrix.length, cols = matrix[0].length;
             int left = 0, right = rows * cols - 1;

             // Perform binary search as if the matrix is a 1D array
             while (left <= right) {
                 int mid = left + (right - left) / 2;
                 int midValue = matrix[mid / cols][mid % cols];
                 
                 if (midValue == target) {
                     return true;
                 } else if (midValue < target) {
                     left = mid + 1;
                 } else {
                     right = mid - 1;
                 }
             }
             
             return false;
         }
     }
     ```
   - **Explanation**: The matrix is treated as a 1D array for the purposes of binary search. Index `mid` is mapped to a 2D position using `mid / cols` (row index) and `mid % cols` (column index).

