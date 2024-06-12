### Sliding Window Approach: Fruit Into Baskets

To solve the problem using the sliding window approach, follow these steps:

1. **Initialization**: Start with two pointers (`start` and `end`) both at the beginning of the array and an empty hashmap to store the count of each type of fruit in the current window.

2. **Expand the Window**: Move the `end` pointer to the right, adding the current fruit to the hashmap. Ensure the hashmap always contains at most two distinct types of fruits.

3. **Shrink the Window**: If the hashmap contains more than two types of fruits, increment the `start` pointer to reduce the window size until there are at most two distinct types of fruits in the hashmap.

4. **Track Maximum**: Throughout the process, track the maximum size of the window.

### Identifying the Sliding Window Approach

This problem fits the sliding window approach because:
- You need to find a contiguous subarray (window) that meets a condition.
- The window can expand and contract dynamically.
- The problem involves finding the maximum size of such a window under given constraints.

### Time and Space Complexity

- **Time Complexity**: O(n), where `n` is the length of the array. Each element is processed at most twice (once when expanding the window and once when contracting).
- **Space Complexity**: O(1), since the hashmap will contain at most three keys.

### Java Implementation

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int totalFruit(int[] fruits) {
        int maxFruits = 0;
        int start = 0;
        Map<Integer, Integer> basket = new HashMap<>();
        
        for (int end = 0; end < fruits.length; end++) {
            basket.put(fruits[end], basket.getOrDefault(fruits[end], 0) + 1);
            
            while (basket.size() > 2) {
                basket.put(fruits[start], basket.get(fruits[start]) - 1);
                if (basket.get(fruits[start]) == 0) {
                    basket.remove(fruits[start]);
                }
                start++;
            }
            
            maxFruits = Math.max(maxFruits, end - start + 1);
        }
        
        return maxFruits;
    }
}
```

### Cornel Notes

**Title:** Fruit Into Baskets (Sliding Window Approach)

**Concept:**
- Problem involves collecting fruits from a row of trees with only two baskets.
- Each basket can hold only one type of fruit.
- Need to find the maximum number of fruits that can be collected consecutively with these constraints.

**Steps:**
1. **Initialization**:
   - Two pointers (`start`, `end`).
   - Hashmap to store count of each fruit type in current window.

2. **Expand Window**:
   - Move `end` pointer to the right.
   - Add current fruit to hashmap.

3. **Shrink Window**:
   - If hashmap size exceeds 2, increment `start` pointer.
   - Adjust hashmap to ensure it contains at most 2 distinct fruit types.

4. **Track Maximum**:
   - Keep track of the maximum window size throughout.

**Complexity:**
- **Time**: O(n) - Each element processed twice at most.
- **Space**: O(1) - Hashmap size capped at 2.

**Java Code:**
```java
class Solution {
    public int totalFruit(int[] fruits) {
        int maxFruits = 0;
        int start = 0;
        Map<Integer, Integer> basket = new HashMap<>();
        
        for (int end = 0; end < fruits.length; end++) {
            basket.put(fruits[end], basket.getOrDefault(fruits[end], 0) + 1);
            
            while (basket.size() > 2) {
                basket.put(fruits[start], basket.get(fruits[start]) - 1);
                if (basket.get(fruits[start]) == 0) {
                    basket.remove(fruits[start]);
                }
                start++;
            }
            
            maxFruits = Math.max(maxFruits, end - start + 1);
        }
        
        return maxFruits;
    }
}
```
