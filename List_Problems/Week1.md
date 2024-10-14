Here are the relevant LeetCode problems and their solutions:

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


