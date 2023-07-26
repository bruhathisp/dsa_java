
## Tags:

Array
Majority Element
Sorting
Hash Map
Moore Voting Algorithm
One-Pass Algorithm
Time Complexity Analysis

### Approach 1: Sorting
Intuition:
The intuition behind this approach is that if an element occurs more than n/2 times in the array (where n is the size of the array), it will always occupy the middle position when the array is sorted. Therefore, we can sort the array and return the element at index n/2.

Explanation:
The code begins by sorting the array nums in non-decreasing order using the sort function from the C++ Standard Library. This rearranges the elements such that identical elements are grouped together.
Once the array is sorted, the majority element will always be present at index n/2, where n is the size of the array.
This is because the majority element occurs more than n/2 times, and when the array is sorted, it will occupy the middle position.
The code returns the element at index n/2 as the majority element.
The time complexity of this approach is O(n log n) since sorting an array of size n takes O(n log n) time.

``` java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return nums[n/2];
    }
}
```
### Approach 2: Hash Map
Intuition:
The intuition behind using a hash map is to count the occurrences of each element in the array and then identify the element that occurs more than n/2 times. By storing the counts in a hash map, we can efficiently keep track of the occurrences of each element.

Explanation:

1. The code begins by initializing a hash map m to store the count of occurrences of each element.
It then iterates through the array nums using a for loop.

2. For each element nums[i], it increments its count in the hash map m by using the line m[nums[i]]++;.

If nums[i] is encountered for the first time, it will be added to the hash map with a count of 1.

If nums[i] has been encountered before, its count in the hash map will be incremented by 1.
3. After counting the occurrences of each element, the code updates n to be n/2, where n is the size of the array. This is done to check if an element occurs more than n/2 times, which is the criteria for being the majority element.

4. The code then iterates through the key-value pairs in the hash map using a range-based for loop.

5. For each key-value pair (x.first, x.second), it checks if the count x.second is greater than n.

If the count is greater than n, it means that x.first occurs more than n/2 times, so it returns x.first as the majority element.

If no majority element is found in the hash map, the code returns 0 as the default value.

Note that this will only occur if the input array nums is empty or does not have a majority element.
The time complexity of this approach is O(n) because it iterates through the array once to count the occurrences and then iterates through the hash map, which has a maximum size of the number of distinct elements in the array.

``` java
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }
        
        n = n / 2;
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() > n) {
                return entry.getKey();
            }
        }
        
        return 0;
    }
}
```
### Approach 3: Moore Voting Algorithm
Intuition:
The intuition behind the Moore's Voting Algorithm is based on the fact that if there is a majority element in an array, it will always remain in the lead, even after encountering other elements.

Explanation:
Algorithm:

The Moore Voting Algorithm is a clever way to find the majority element in an array. The majority element is the element that appears more than half of the array's size.

Here's how the algorithm works:

1. Start with two variables, `count` and `candidate`. Set `count` to 0 and `candidate` to an arbitrary value.

2. Go through each element in the array one by one.

3. If `count` is 0, it means we don't have a candidate yet, so we pick the current element as the new candidate and set `count` to 1.

4. If the current element matches the candidate, it means it is reinforcing the majority element, so we increment `count` by 1.

5. If the current element is different from the candidate, it means we have a mix of elements with equal occurrences, so we decrement `count` by 1.

6. The algorithm continues this process until it has gone through the entire array.

7. The final value of the `candidate` variable will hold the majority element.

The algorithm's cleverness lies in the fact that if the majority element appears more than half of the time in the array, it will always have a lead in the `count` variable. Even if other elements decrease the `count`, the majority element will eventually regain the lead.

It's important to note that the algorithm assumes the majority element exists in the array, which is stated in the problem.

The time complexity of the Moore Voting Algorithm is O(n) since it only traverses the array once. This makes it very efficient compared to sorting or using a hash map.

The provided Java code implements the Moore Voting Algorithm to find the majority element in the `nums` array and returns the result.

This approach is efficient compared to sorting as it requires only a single pass through the array and does not change the original order of the elements.

``` java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;
        
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        
        return candidate;
    }
}
```
