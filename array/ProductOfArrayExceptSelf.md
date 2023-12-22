# Solution

## Method

### Find the product of elements left to the element -->  Find the product of elements right to the element--> Store in the array
### Multiply them both

NOTES

1. use `pre` to store the left product
2. reset `pre` to `
3. use `pre` to store the right product
4. In ans array update the left product for corresponding element and in another loop the same array is updated for right product

Let's go step by step through the algorithm with the input `nums = [1,2,3,4]`:

1. **Forward Iteration:**
   - Initialize `pre` to 1.
   - Loop through the array from left to right.
   - For each element, update `ans[i]` to be the product of all elements to the left of `i` (excluding `nums[i]`).
   - Update `pre` by multiplying it by the current element.

   | Iteration | `i` | `nums[i]` | `pre` | `ans`               |
   |-----------|-----|-----------|-------|---------------------|
   | 1         | 0   | 1         | 1     | [1, 1, 1, 1]        |
   | 2         | 1   | 2         | 1     | [1, 1, 1, 1]        |
   | 3         | 2   | 3         | 2     | [1, 1, 2, 1]        |
   | 4         | 3   | 4         | 6     | [1, 1, 2, 6]        |

2. **Backward Iteration:**
   - Reset `pre` to 1.
   - Loop through the array from right to left.
   - For each element, update `ans[i]` by multiplying it with the product of all elements to the right of `i` (excluding `nums[i]`).
   - Update `pre` by multiplying it by the current element.

   | Iteration | `i` | `nums[i]` | `pre` | `ans`               |
   |-----------|-----|-----------|-------|---------------------|
   | 4         | 3   | 4         | 1     | [24, 12, 8, 6]      |
   | 3         | 2   | 3         | 4     | [24, 12, 8, 6]      |
   | 2         | 1   | 2         | 12    | [24, 12, 8, 6]      |
   | 1         | 0   | 1         | 24    | [24, 12, 8, 6]      |

3. **Result:**
   - The final `ans` array is `[24, 12, 8, 6]`, where each element is the product of all elements except the one at that index.

So, the algorithm efficiently computes the product of all elements except the current one without using the division operation and runs in O(n) time complexity.





## Code
``` java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int ans[] = new int[nums.length];
        int pre=1;
        for(int i=0; i<nums.length; i++ ){
        // not reverse order to calculate left product

        
            ans[i]=pre;
            pre*=nums[i];
        }
        pre =1;
        for(int i=nums.length-1; i>=0; i--){
// reverse order to calculate right product

            ans[i]*=pre;
            pre*=nums[i];
        }
        return ans;
    }
}
```
