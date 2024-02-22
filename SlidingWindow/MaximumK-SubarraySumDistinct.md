## Solution

[Question](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)


### Difference between Hashset and Hashmap and why we use hashmap in this probem

| HashSet Example | HashMap Example |
|---|---|
|
```java
import java.util.HashSet;

// Creating a HashSet
HashSet<Integer> set = new HashSet<>();

// Adding elements to the HashSet
set.add(1);
set.add(2);
set.add(3);

// Removing an element from the HashSet
set.remove(2);

// Checking if an element exists in the HashSet
boolean contains = set.contains(3);

// Iterating over elements in the HashSet
for (Integer num : set) {
    System.out.println(num);
}

// Getting the size of the HashSet
int size = set.size();

// Clearing the HashSet
set.clear();
```
|
```java
import java.util.HashMap;

// Creating a HashMap
HashMap<Integer, String> map = new HashMap<>();

// Adding key-value pairs to the HashMap
map.put(1, "One");
map.put(2, "Two");
map.put(3, "Three");

// Removing a key-value pair from the HashMap
map.remove(2);

// Getting the value associated with a key
String value = map.get(1);

// Checking if a key exists in the HashMap
boolean containsKey = map.containsKey(3);

// Iterating over key-value pairs in the HashMap
for (Integer key : map.keySet()) {
    String val = map.get(key);
    System.out.println(key + " -> " + val);
}

// Getting the size of the HashMap
int size = map.size();

// Clearing the HashMap
map.clear();
```
 |




- Both `Set` and `Map` are collections in Java's Collections Framework.
- `Set` stores unique elements and does not allow duplicates.
- `Map` maps keys to values and allows associating each key with a value.
- In this problem, we need to keep track of both elements and their frequencies within the sliding window.
- Using a `Map` allows efficient tracking of both elements and their frequencies, aiding in checking for distinctness within the window.
- Therefore, we use a `Map` instead of a `Set` in this problem to efficiently handle the distinct element condition while also tracking element frequencies.


## Explanation

1. **Understanding the Problem**: The problem asks for finding the maximum sum of distinct subarrays of length k within the given array.

2. **Sliding Window Approach**: Since we need to find subarrays of a fixed length k and maximize their sum, we can use a sliding window approach. This involves maintaining a window of size k and moving it along the array, updating the sum and other necessary data structures as we go.

3. **Iterating Through the Array**: We iterate through the array using two pointers, `start` and `end`, denoting the start and end indices of the sliding window. At each step, we update the frequency map and adjust the window as needed.

4. **Adjusting the Window**: While iterating, if we encounter an element that violates the distinctness condition (i.e., its frequency within the window exceeds 1), or if the window size exceeds k, we shrink the window from the left by incrementing the `start` pointer and updating the sum accordingly. We continue this process until the window satisfies the conditions.

5. **Updating Maximum Sum**: Whenever the window size reaches k, we update the maximum sum if the current sum is greater than the existing maximum sum. However, if the size of the current window is not equal to `k`, it means the current window doesn't represent a subarray of the desired length. In this case, the code simply continues without updating the `maxSum`.

6. **Returning the Result**: Once we've iterated through the entire array, we return the maximum sum found.

By following this thought process and implementing the sliding window technique with appropriate adjustments for the distinctness condition, we can efficiently solve the problem and find the maximum sum of distinct subarrays of length k within the given array.


## Adjusting the window

```java
while (frequencyMap.get(nums[end]) > 1 || end - start + 1 > k) {
    frequencyMap.put(nums[start], frequencyMap.get(nums[start]) - 1);
    if (frequencyMap.get(nums[start]) == 0) {
        frequencyMap.remove(nums[start]);
    }
    sum -= nums[start];
    start++;
}
```

1. **Condition**: This `while` loop is responsible for adjusting the sliding window when it violates the conditions:

    - `frequencyMap.get(nums[end]) > 1`: If the frequency of the current element (`nums[end]`) within the window exceeds 1, it means the element is repeated within the window, violating the distinctness condition.
    
    - `end - start + 1 > k`: If the window size exceeds k, it's also violating the condition of having a window of length k.

    We need to adjust the window until both of these conditions are satisfied.

2. **Adjusting the Window**:
   
    - `frequencyMap.put(nums[start], frequencyMap.get(nums[start]) - 1)`: Decrements the frequency of the element at the start of the window (`nums[start]`) since it's going out of the window.
   
    - `if (frequencyMap.get(nums[start]) == 0) { frequencyMap.remove(nums[start]); }`: If the frequency of the element at the start becomes 0 after decrementing, it means that element is no longer present in the window. In this case, we remove it from the frequency map to keep track of only the elements within the window.
   
    - `sum -= nums[start]`: Subtracts the element at the start of the window from the current sum since it's going out of the window.
   
    - `start++`: Moves the start pointer one step forward to shrink the window from the left.

3. **Continuing the Process**:
   
    - We continue this process until both conditions (`frequencyMap.get(nums[end]) > 1` and `end - start + 1 > k`) are no longer true. This ensures that we adjust the window until it satisfies the conditions of distinctness and the desired length (k).

4. **Result**:
   
    - After the `while` loop, the window will have been adjusted to satisfy the conditions, and we can proceed with the next iteration or compute the maximum sum if the window size equals k.
## Solution

``` java
  import java.util.HashMap;
import java.util.Map;

class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        int start = 0;
        long sum = 0;
        long maxSum = 0;

        for (int end = 0; end < nums.length; end++) {
            frequencyMap.put(nums[end], frequencyMap.getOrDefault(nums[end], 0) + 1);

            while (frequencyMap.get(nums[end]) > 1 || end - start + 1 > k) {
                frequencyMap.put(nums[start], frequencyMap.get(nums[start]) - 1);
                if (frequencyMap.get(nums[start]) == 0) {
                    frequencyMap.remove(nums[start]);
                }
                sum -= nums[start];
                start++;
            }

            sum += nums[end];

            if (end - start + 1 == k) {
                maxSum = Math.max(maxSum, sum);
            }
        }

        return maxSum;
    }
}

```


```
int[] nums = {1, 1, 1, 7, 8, 9};
int k = 3;

Current Window: [0, 0]
Current Window Elements: [1]
Frequency Map: {1=1}
Current Sum: 1

Current Window: [0, 1]
Current Window Elements: [1, 1]
Frequency Map: {1=2}
Adjusting Window: [1, 1]
Adjusted Window Elements: [1]
Frequency Map: {1=1}
Current Sum: 1

Current Window: [1, 2]
Current Window Elements: [1, 1]
Frequency Map: {1=2}
Adjusting Window: [2, 2]
Adjusted Window Elements: [1]
Frequency Map: {1=1}
Current Sum: 1

Current Window: [2, 3]
Current Window Elements: [1, 7]
Frequency Map: {1=1, 7=1}
Current Sum: 8

Current Window: [2, 4]
Current Window Elements: [1, 7, 8]
Frequency Map: {1=1, 7=1, 8=1}
Current Sum: 16
Maximum Sum Updated: 16

Current Window: [2, 5]
Current Window Elements: [1, 7, 8, 9]
Frequency Map: {1=1, 7=1, 8=1, 9=1}
Adjusting Window: [3, 5]
Adjusted Window Elements: [7, 8, 9]
Frequency Map: {7=1, 8=1, 9=1}
Current Sum: 24
Maximum Sum Updated: 24

Result: 24
```


## Code to print the above 

``` java
import java.util.HashMap;
import java.util.Map;
import java.util.Arrays;

class MyClass {
    public long maximumSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        int start = 0;
        long sum = 0;
        long maxSum = 0;

        for (int end = 0; end < nums.length; end++) {
            frequencyMap.put(nums[end], frequencyMap.getOrDefault(nums[end], 0) + 1);
            System.out.println("Current Window: [" + start + ", " + end + "]");
            System.out.println("Current Window Elements: " + Arrays.toString(Arrays.copyOfRange(nums, start, end+1)));
            System.out.println("Frequency Map: " + frequencyMap);

            while (frequencyMap.get(nums[end]) > 1 || end - start + 1 > k) {
                frequencyMap.put(nums[start], frequencyMap.get(nums[start]) - 1);
                if (frequencyMap.get(nums[start]) == 0) {
                    frequencyMap.remove(nums[start]);
                }
                sum -= nums[start];
                start++;
                System.out.println("Adjusting Window: [" + start + ", " + end + "]");
                System.out.println("Adjusted Window Elements: " + Arrays.toString(Arrays.copyOfRange(nums, start, end+1)));
                System.out.println("Frequency Map: " + frequencyMap);
            }

            sum += nums[end];
            System.out.println("Current Sum: " + sum);

            if (end - start + 1 == k) {
                maxSum = Math.max(maxSum, sum);
                System.out.println("Maximum Sum Updated: " + maxSum);
            }
            
            System.out.println();
        }

        return maxSum;
    }
    
    public static void main(String[] args) {
        MyClass myClass = new MyClass();

        int[] nums = {1, 1, 1, 7, 8, 9};
        int k = 3;

        long result = myClass.maximumSubarraySum(nums, k);
        System.out.println("Result: " + result);
    }
}


```
