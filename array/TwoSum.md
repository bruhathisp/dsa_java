The Two Sum problem is a classic programming problem that involves finding two numbers in an array that add up to a given target value. It's often used as an interview question and is a good exercise for practicing problem-solving skills. The task is to return the indices of the two numbers that form the target sum.
Approach 1: Brute Force
The first solution provided is a brute force approach. It uses nested loops to consider every pair of elements in the array and checks if their sum equals the target. The time complexity of this approach is O(n^2), where n is the number of elements in the array.
'''java 
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
} '''
Approach 2: Two-pass Hash Table
The second solution uses a two-pass hash table (or unordered_map in C++). In the first pass, it builds a hash table that maps each element of the array to its index. Then, in the second pass, it iterates through the array and checks if the complement (target minus the current element) exists in the hash table. If found, it returns the indices of the two numbers. This approach has a time complexity of O(n) since hash table lookups take constant time on average.

'''java 
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
} '''
Approach 3: One-pass Hash Table
The third solution is an optimized version of the second approach. Instead of using two passes, it employs a one-pass hash table. While iterating through the array, it calculates the complement for each element and checks if it already exists in the hash table. If found, it returns the indices of the two numbers. If not found, it adds the current element and its index to the hash table. This approach also has a time complexity of O(n) and is more efficient than the brute force method.

'''java 
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
'''
When to Choose Each Approach:
Brute Force: The brute force approach can be chosen when you don't have any constraints on the time complexity, or the array size is relatively small. It's straightforward to implement but becomes inefficient for large arrays due to its O(n^2) time complexity.
Two-pass Hash Table: The two-pass hash table approach is suitable when the array is not sorted, and you need to return the indices of the two numbers that add up to the target. This approach ensures O(n) time complexity and is more efficient than the brute force approach.
One-pass Hash Table: The one-pass hash table approach is the most efficient and recommended solution for the Two Sum problem. It works for both sorted and unsorted arrays, and it guarantees O(n) time complexity. This approach should be preferred in most scenarios.
It's essential to consider the problem's specific requirements and constraints when choosing the appropriate solution. The one-pass hash table approach is widely used in real-world applications for its efficiency and simplicity.




