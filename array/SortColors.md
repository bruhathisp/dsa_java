##Solution


Lessons Learnt:

1. Dutch National Flag algorithm or one-pass algorithm or 3-way partitioning.
2. To print array with placeholders like this `System.out.printf("%d %d %d %s\n", low, mid, high, Arrays.toString(nums));`
3. To iterate through each element in a HashMap by using `Map.Entry<Integer, Integer> entry : map.entrySet()`

The first method is similar to the majority element sum. Because I used `map.getOrDefault(nums[i], 0) + 1)` to count the occurences of the element in the HashMap



## Method 1  Time complexity  O(N + K)

Notes for time complexity

In practice, if the number of unique elements K is much smaller than N, the term O(K) becomes less significant compared to O(N), and the method behaves more like O(N). 


Steps: 
1. Put elements into Hashmap while counting with the `getOrDefault()` method
2. Use this `Map.Entry<Integer, Integer> entry : map.entrySet()` to iterate through each element `0, 1 and 2` in the HashMap
3. Update the nums array and n value.

1. Enter the array elements and its corresponding count. 

``` java
    class Solution {
    public void sortColors(int[] nums) {

        Map<Integer,Integer> map= new HashMap();

        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);            
        }

        int n=0;
        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
                for ( int i=0;  i < entry.getValue(); i++) {
                      nums[n] = entry.getKey();
                      n++;
            }

        }
    }
}

```


## Method 2

### Dutch National Flag algorithm or one-pass algorithm or 3-way partitioning.

It is an algorithm for sorting an array containing three distinct values. I am mind-blown with how this algo is implemented.

The Dutch National Flag algorithm is called one-pass because it sorts the array in a single pass through the elements. The time complexity of the algorithm is O(n), where n is the size of the array.

Complexity
Time complexity: O(n)
Space complexity: O(1)

``` java
class Solution {
    public void sortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.length - 1;
        while (mid <= high) {
            if (nums[mid] == 0) {
                swap(nums, low, mid);
                low++;
                mid++;
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                swap(nums, mid, high);
                high--;
            }
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

```



Steps: Write it down a note while revising, you will understand it better.

1. Initiate  ` low = 0, mid = 0, high = nums.length - 1` and iterate till `mid<=high`
2. If value at `mid =0` swap the low and mid. Then increment both
3. If value at `mid =1` don't do anything and just increment the mid.
4. If value at `mid =2` swap the high and mid. Then decrement only high.

Input:  [1, 0, 1, 1, 0, 2, 0]
Expected Output: [0, 0, 0, 1, 1, 1, 2]

| Iteration | `low` | `mid` | `high` | `nums`                   |
|-----------|-------|-------|--------|--------------------------|
| 0         | 0     | 1     | 6      | [1, 0, 1, 1, 0, 2, 0]    |
| 1         | 0     | 2     | 6      | [0, 1, 1, 1, 0, 2, 0]    |
| 2         | 1     | 3     | 6      | [0, 1, 1, 1, 0, 2, 0]    |
| 3         | 1     | 4     | 6      | [0, 1, 1, 1, 0, 2, 0]    |
| 4         | 2     | 5     | 6      | [0, 0, 1, 1, 1, 2, 0]    |
| 5         | 2     | 6     | 5      | [0, 0, 1, 1, 1, 0, 2]    |
| 6         | 3     | 6     | 5      | [0, 0, 0, 1, 1, 1, 2]    |
| Result    |       |       |        | [0, 0, 0, 1, 1, 1, 2]    |

The `low`, `mid`, `high`, and `nums` columns represent the values after each iteration of the while loop. 

![image](https://github.com/bruhathisp/dsa_java/assets/91585301/c148fb56-6dfb-45db-b90e-28f9b8acd5d9)




