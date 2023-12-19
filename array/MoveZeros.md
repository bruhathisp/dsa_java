# Solution


The code has a time complexity of O(n) and space complexity of O(1), where n is the length of the nums array.

``` java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0;
        for (int num : nums) {
            if (num != 0) {
                nums[i] = num;
                i++;
            }
        }

        while (i <= nums.length - 1) {
            nums[i] = 0;
            i++;
        }
    }
}
```
