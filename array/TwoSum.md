The Two Sum problem is a classic programming problem that involves finding two numbers in an array that add up to a given target value. It's often used as an interview question and is a good exercise for practicing problem-solving skills. The task is to return the indices of the two numbers that form the target sum.


Lessons Learnt:
1. How to create a Hash Table (or unordered map in C++) `numMap.put(nums[i], i);` --> key, value --> element, index
2. We can use `containsKey` and `get` method for Hash Table `numMap.containsKey(complement) && numMap.get(complement) != i`

## Approach 1: Brute Force
The first solution provided is a brute force approach. It uses nested loops to consider every pair of elements in the array and checks if their sum equals the target. The time complexity of this approach is O(n^2), where n is the number of elements in the array.
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{}; // No solution found
    }
}
```
## Approach 2: Two-pass Hash Table: Time complexity of O(n)
The second solution uses a two-pass hash table (or unordered_map in C++). 

In the first pass, it **builds a hash table** `(element, index)` that maps each element of the array to its index. 

Then, in the second pass, it iterates through the array and **checks if the complement** (target minus the current element) exists in the hash table and **not equal to the current element**. 

If found, it returns the indices of the two numbers. 

```java 
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numMap = new HashMap<>();
        int n = nums.length;
        // Build the hash table
        for (int i = 0; i < n; i++) {
            numMap.put(nums[i], i);
        }
        // Find the complement
        for (int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if (numMap.containsKey(complement) && numMap.get(complement) != i) {
                return new int[]{i, numMap.get(complement)};
            }
        }

        return new int[]{}; // No solution found
    }
}
```
**for (int i = 0; i < n; i++) { numMap.put(nums[i], i); }: **

This loop iterates through the nums array, and for each element nums[i], it adds a key-value pair to the numMap where **the key is the element itself (nums[i])** and the value is its index (i).
This effectively creates a hash table with the elements of the nums array and their indices for easy lookup.

**if (numMap.containsKey(complement) && numMap.get(complement) != i) { ... }: **

The code checks if the complement exists in the numMap and ensures that the index of the complement is not equal to the current index i. This condition is necessary to avoid using the same element twice in the solution.

**return new int[]{i, numMap.get(complement)};: **

If a valid pair is found (i.e., the complement exists in the numMap and has a different index), the function returns an array containing the indices i and numMap.get(complement), representing the indices of the two elements whose sum is equal to the target.


## Approach 3: One-pass Hash Table
The third solution is an optimized version of the second approach. Instead of using two passes, it employs a one-pass hash table. While iterating through the array, it calculates the complement for each element and checks if it already exists in the hash table. If found, it returns the indices of the two numbers. If not found, it adds the current element and its index to the hash table. This approach also has a time complexity of O(n) and is more efficient than the brute force method.

```java 
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numMap = new HashMap<>();
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if (numMap.containsKey(complement)) {
                return new int[]{numMap.get(complement), i};
            }
            numMap.put(nums[i], i);
        }

        return new int[]{}; // No solution found
    }
}
```
When to Choose Each Approach:

Brute Force: The brute force approach can be chosen when you don't have any constraints on the time complexity, or the array size is relatively small. It's straightforward to implement but becomes inefficient for large arrays due to its O(n^2) time complexity.

Two-pass Hash Table: The two-pass hash table approach is suitable when the array is not sorted, and you need to return the indices of the two numbers that add up to the target. This approach ensures O(n) time complexity and is more efficient than the brute force approach.

One-pass Hash Table: The one-pass hash table approach is the most efficient and recommended solution for the Two Sum problem. It works for both sorted and unsorted arrays, and it guarantees O(n) time complexity. This approach should be preferred in most scenarios.

It's essential to consider the problem's specific requirements and constraints when choosing the appropriate solution. The one-pass hash table approach is widely used in real-world applications for its efficiency and simplicity.




