##Solution

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



