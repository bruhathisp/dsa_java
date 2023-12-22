# Solution


Overview 

Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]

The distinct triplets are [-1,0,1] and [-1,-1,2].

Notice that the order of the output and the order of the triplets does not matter.

Notes:

There is this interesting way to skip the duplicates during the calculation which increases the efficiency.
1. `if (i == 0 || (i > 0 && nums[i] != nums[i - 1]))`  eliminates duplicates with the first element
2. `while (left < right && nums[left] == nums[left + 1]) left++; `  Skip duplicates for the other elements
3.  We use the target as it is the negative of (left+right) pointer element values.

## Method 1
``` java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {
            if (i == 0 || (i > 0 && nums[i] != nums[i - 1])) {
            // eliminate duplicates with the first element
                int target = -nums[i];
                int left = i + 1, right = nums.length - 1;

                while (left < right) {
                    int sum = nums[left] + nums[right];

                    if (sum == target) {
                        result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                        while (left < right && nums[left] == nums[left + 1]) left++;   // Skip duplicates for the second element
                        while (left < right && nums[right] == nums[right - 1]) right--;   // Skip duplicates for the third element

                        left++;
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }

        return result;
    }
}
```
## Explanation
Let's go through the first example `nums = [-1,0,1,2,-1,-4]` and see how the algorithm works step by step:

1. **Sort the array:** `[-4, -1, -1, 0, 1, 2]`

2. **Iterate through the array:** Start with the first element (`-4` in this case), and use two pointers (`left` and `right`) to find triplets that sum up to zero.

   - **First Iteration (i=0, nums[i] = -4):**
     - `left = 1`, `right = 5`, `target = 4`
     - Since `-4 + -1 + 2 = -3` is less than `target`, move `left` pointer to the right.
     - Update `left = 2`, `right = 5`, `target = 4`
     - Since `-4 + -1 + 2 = -3` is still less than `target`, move `left` pointer to the right.
     - Update `left = 3`, `right = 5`, `target = 4`
     - Now, `-4 + 0 + 2 = -2` is less than `target`, move `left` pointer to the right.
     - Update `left = 4`, `right = 5`, `target = 4`
     - Now, `-4 + 1 + 2 = -1` is less than `target`, move `left` pointer to the right.
     - Update `left = 5`, `right = 5`, `target = 4`
     - Now, `-4 + 2 + 2 = 0` is equal to `target`, add the triplet `[-4, -1, 2]` to the result set.
     - Skip duplicates: Increment `left` to skip duplicate elements.
     - Update `left = 6`, `right = 5`, `target = 4`
     - Now, `left` is greater than `right`, move to the next element.

   - **Second Iteration (i=1, nums[i] = -1):**
     - Since the array is sorted, we can skip duplicates, so move to the next element.

   - **Third Iteration (i=2, nums[i] = -1):**
     - `left = 3`, `right = 5`, `target = 2`
     - Since `-1 + 0 + 2 = 1` is less than `target`, move `left` pointer to the right.
     - Update `left = 4`, `right = 5`, `target = 2`
     - Now, `-1 + 1 + 2 = 2` is equal to `target`, add the triplet `[-1, 0, 1]` to the result set.
     - Skip duplicates: Increment `left` to skip duplicate elements.
     - Update `left = 6`, `right = 5`, `target = 2`
     - Now, `left` is greater than `right`, move to the next element.

   - **Fourth Iteration (i=3, nums[i] = 0):**
     - Since the array is sorted, we can skip duplicates, so move to the next element.

   - **Fifth Iteration (i=4, nums[i] = 1):**
     - Since the array is sorted, we can skip duplicates, so move to the next element.

   - **Sixth Iteration (i=5, nums[i] = 2):**
     - Since the array is sorted, we can skip duplicates, so move to the next element.

3. **Final Result Set:** `[[ -4, -1, 2], [-1, 0, 1]]`

This ensures that the solution set doesn't contain duplicate triplets, and the order of the output and the order of the triplets doesn't matter.



## Method 2


There is this most efficient implementation with abstract classes and Two Sum Method helping to find the target variable. I'll further write the explanation when I revisit the hard part of the array.



``` java

import java.util.*;

class Solution {
    private List<List<Integer>> res;

    public List<List<Integer>> threeSum(int[] nums) {
        return new AbstractList<List<Integer>>() {

            public List<Integer> get(int index) {
                init();
                return res.get(index);
            }

            public int size() {
                init();
                return res.size();
            }

            private void init() {
                if (res != null) return;
                Arrays.sort(nums);
                ArrayList<List<Integer>> ans = new ArrayList<>();

                for (int i = 0; i < nums.length; i++) {
                    if (i != 0 && nums[i] == nums[i - 1]) continue; // Avoid repetition
                    twoSum(i, ans);
                }
                res = ans;
            }

            private void twoSum(int i, ArrayList<List<Integer>> ans) {
                int target = -nums[i], left = i + 1, right = nums.length - 1;

                while (left < right) {
                    //Skip repeating second number
                    if (i + 1 < left && nums[left - 1] == nums[left]) {
                        left++;
                        continue;
                    }
                    if (nums[left] + nums[right] > target) right--;
                    else if (nums[left] + nums[right] < target) left++;
                    else {
                        ArrayList<Integer> l = new ArrayList<>(3); // 3 is the size of list
                        l.add(nums[i]);
                        l.add(nums[left]);
                        l.add(nums[right]);
                        ans.add(l);
                        left++; // move to next pair
                    }
                }
            }
        };
    }
}

```
