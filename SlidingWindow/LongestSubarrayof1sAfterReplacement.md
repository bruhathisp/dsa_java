[Question](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/description/)

You are given a binary array nums, meaning the array consists only of 0s and 1s. You need to delete exactly one element from the array. Your task is to find the length of the longest subarray that contains only 1s after performing this deletion.The idea is to find the longest subarray of 1s that can be formed by removing exactly one element (0). Here's how we can achieve that:

### Approach:

1. **Initialization**:
   - Use two pointers `left` and `right` to represent the sliding window.
   - Use a variable `zeroCount` to keep track of the number of 0s in the current window.
   - Use a variable `maxLength` to store the length of the longest subarray of 1s found so far.

2. **Expand the Window**:
   - Move the `right` pointer to the right, expanding the window.
   - If the current element is 0, increment `zeroCount`.

3. **Shrink the Window**:
   - If `zeroCount` exceeds 1, it means we have more than one 0 in the window, which is not allowed. Move the `left` pointer to the right until `zeroCount` is less than or equal to 1.

4. **Update the Result**:
   - Update `maxLength` with the length of the current window minus one (since we need to remove one element).

5. **Return the Result**:
   - Return `maxLength`.

### Java Implementation:

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int left = 0, right = 0;
        int zeroCount = 0;
        int maxLength = 0;

        while (right < nums.length) {
            if (nums[right] == 0) {
                zeroCount++;
            }
            
            while (zeroCount > 1) {
                if (nums[left] == 0) {
                    zeroCount--;
                }
                left++;
            }

            // We subtract 1 because we must delete one element
            maxLength = Math.max(maxLength, right - left);
            right++;
        }

        return maxLength;
    }
}
```

### Explanation:

1. **Initialization**:
   - `left = 0, right = 0` — Initialize the sliding window pointers.
   - `zeroCount = 0` — Initialize the count of 0s in the current window.
   - `maxLength = 0` — Initialize the maximum length of the subarray of 1s.

2. **Expand the Window**:
   - Iterate through the array with the `right` pointer.
   - If the current element is 0, increment `zeroCount`.

3. **Shrink the Window**:
   - If `zeroCount` exceeds 1, move the `left` pointer to the right and decrement `zeroCount` if the element at `left` is 0.

4. **Update the Result**:
   - Update `maxLength` with the length of the current window minus one.

5. **Return the Result**:
   - Return the value of `maxLength`.

### Examples:

- **Example 1**:
  - Input: `nums = [1,1,0,1]`
  - Output: `3`
  - Explanation: After deleting the 0 at position 2, the array becomes `[1,1,1]` which has a length of 3.

- **Example 2**:
  - Input: `nums = [0,1,1,1,0,1,1,0,1]`
  - Output: `5`
  - Explanation: After deleting the 0 at position 4, the array becomes `[0,1,1,1,1,1,0,1]` which has a longest subarray of 1s with length 5.

- **Example 3**:
  - Input: `nums = [1,1,1]`
  - Output: `2`
  - Explanation: You must delete one element, so the longest subarray of 1s after deleting any one element is of length 2.

This sliding window approach ensures an efficient solution with a time complexity of O(n), where n is the length of the array.

## example 2 


### Detailed Example of Sliding Window Approach

**Example**:
- Input: `nums = [0, 1, 1, 1, 0, 1, 1, 0, 1]`

**Steps**:

1. Initialize:
   - `left = 0, right = 0`
   - `zeroCount = 0`
   - `maxLength = 0`

2. Expand the window:

   - `right = 0`: `nums[right] = 0`
     - `zeroCount = 1`
     - Valid window: `[0]` (length: 1 - 1 = 0)
    
     - `nums = [0, 1, 1, 1, 0, 1, 1, 0, 1]`

   - `right = 1`: `nums[right] = 1`
     - `zeroCount = 1`
     - Valid window: `[0, 1]` (length: 2 - 1 = 1)
     - `maxLength = 1`
    
     - `nums = [0, 1, 1, 1, 0, 1, 1, 0, 1]`

   - `right = 2`: `nums[right] = 1`
     - `zeroCount = 1`
     - Valid window: `[0, 1, 1]` (length: 3 - 1 = 2)
     - `maxLength = 2`

   - `right = 3`: `nums[right] = 1`
     - `zeroCount = 1`
     - Valid window: `[0, 1, 1, 1]` (length: 4 - 1 = 3)
     - `maxLength = 3`

   - `right = 4`: `nums[right] = 0`
     - `zeroCount = 2`
     - Invalid window (more than 1 zero)

3. Shrink the window:   `nums = [0, 1, 1, 1, 0, 1, 1, 0, 1]`
   - Move `left` to 1: `nums[left] = 0`
     - `zeroCount = 1`
     - Valid window: `[1, 1, 1, 0]` (length: 3)

4. Continue expanding and shrinking:  `nums = [0, 1, 1, 1, 0, 1, 1, 0, 1]`
   - `right = 5`: `nums[right] = 1`
     - `zeroCount = 1`
     - Valid window: `[1, 1, 1, 0, 1]` (length: 4)
     - `maxLength = 4`

   - `right = 6`: `nums[right] = 1`
     - `zeroCount = 1`
     - Valid window: `[1, 1, 1, 0, 1, 1]` (length: 5)
     - `maxLength = 5`

   - `right = 7`: `nums[right] = 0`
     - `zeroCount = 2`
     - Invalid window

5. Shrink the window again:
   - Move `left` to 2: `nums[left] = 1`
     - `zeroCount = 2`
   - Move `left` to 3: `nums[left] = 1`
     - `zeroCount = 2`
   - Move `left` to 4: `nums[left] = 1`
     - `zeroCount = 1`
     - Valid window: `[0, 1, 1]` (length: 2)

6. Continue expanding:
   
