# Solution


[Question](https://leetcode.com/problems/3sum/description/)

Overview 

Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]

The distinct triplets are [-1,0,1] and [-1,-1,2].

Notice that the order of the output and the order of the triplets does not matter.

To skip duplicates, you need to check if the current element is the same as the previous element (not the next one) to ensure you're not processing the same element twice.

Here are the necessary corrections to your `threeSum` function to ensure it handles duplicates correctly and calculates the triplets as expected.

### Key Corrections Needed:

1. **Check for duplicates before entering the while loop:**
   - Skip duplicate elements for `i` correctly with the condition `if (i > 0 && nums[i] == nums[i - 1]) continue;`.

2. **Position of duplicate check for `j` and `k`:**
   - Move the duplicate skipping logic for `j` and `k` inside the while loop after adjusting `j` and `k`.





```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> set = new ArrayList<>();

        Arrays.sort(nums); // Sort the array first

        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue; // Skip duplicates for i

            int j = i + 1;
            int k = nums.length - 1;

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    set.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    k--;
                    while (j < k && nums[j] == nums[j - 1]) j++; // Skip duplicates for j
                    while (j < k && nums[k] == nums[k + 1]) k--; // Skip duplicates for k
                } else if (sum < 0) {
                    j++;
                } else {
                    k--;
                }
            }
        }

        return set;
    }
}
```

### Detailed Explanation of Corrections:

1. **Initialization and Sorting:**
   - The array `nums` is sorted to simplify finding the triplets and handling duplicates.

2. **Outer Loop for `i`:**
   - The loop runs from `0` to `nums.length - 2` because we need at least three numbers to form a triplet.
   - The condition `if (i > 0 && nums[i] == nums[i - 1]) continue;` ensures that duplicate elements for `i` are skipped, preventing duplicate triplets.

3. **Two-Pointer Technique:**
   - For each `i`, initialize `j` to `i + 1` and `k` to the end of the array (`nums.length - 1`).
   - Use a while loop to move `j` and `k` towards each other, calculating the sum of `nums[i] + nums[j] + nums[k]`.

4. **Checking and Adjusting Pointers:**
   - If the sum is zero, add the triplet to the result and move both pointers inward while skipping duplicates.
   - If the sum is less than zero, increment `j` to increase the sum.
   - If the sum is greater than zero, decrement `k` to decrease the sum.
   - After adjusting `j` and `k`, the while loop ensures that duplicates are skipped for both `j` and `k`.

