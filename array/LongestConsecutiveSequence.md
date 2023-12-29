## Solution



### Union Find ---> Time Complexity O(n)
1. Create a parent array with the index of the nums array. The corresponding array element is its own parent in the initial stage. So in this example ` nums= [ 100 4 200 1 3 2]` and ` parents= [ 0 1 2 3 4 5]`
2. Create a hashmap `size` where the elements are the key and their index as the values.
3. Take two terms, `nums[i]-1` and `nums[i]+1` if the size map has the values, union the values `nums[i] and nums[i-1]` and `nums[i] and nums[i+1]` respectively.
4. In union method, the parent array of the neighbour value is updated to the value of the parent itself.
5. How do you find the value of the parent node? See the explanation below.
6. Final parent array is obtained.
7. Then we calculate the maximum consecutive sequence by looking up the parent array. More explanation given below.
   
   




``` java
class Solution {
    public int longestConsecutive(int[] nums) {
        
        
        int[] parent = new int[nums.length];
        for(int i = 0; i < nums.length; i++) parent[i] = i;
        HashMap<Integer, Integer> size = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(size.containsKey(nums[i])) continue;
            size.put(nums[i], i);
        }
        for(int i = 0; i < nums.length; i++){
            if(size.containsKey(nums[i] - 1) ){
                union(parent, i, size.get(nums[i] - 1));
            }
            if(size.containsKey(nums[i] + 1)){
                union(parent, i, size.get(nums[i] + 1));
            }
        }
        //return max 
        int res = 0;
        int findValue=0;
        int[] count = new int[parent.length];
        for(int value : size.values()){
            findValue = find(parent, value);
            count[findValue] ++;
            res = Math.max(res, count[findValue]);
        }
        return res;
    }
    
    
    public void union(int[] parent, int a, int b){
        int parent_of_a = find(parent, a);
        int parent_of_b = find(parent, b);
        parent[parent_of_a] = parent_of_b;
    }
    public int find(int[] parent, int c){
        while(parent[c] != c){
            parent[c] = parent[parent[c]]; // Path compression
            c = parent[c];
        }
        return c;
    }
}


```


### Updating parent array
Let's go through the code step by step and create a table to keep track of the parameters and function calls. The input array is [100, 4, 200, 1, 3, 2], and the expected output is 4.

| Iteration | i  | nums[i] |  size | Union Operation (nums[i] - 1) | Updated Parent Array | Union Operation (nums[i] + 1) | Updated Parent Array | 
|-----------|----|---------|-----------------------------|-----------------------|------|-----------------------------|-----------------------|
| 1         | 0  | 100     |  {100=0, 4=5, 200=2, 1=3, 3=4, 2=5} | -                           | [0, 1, 2, 3, 4, 5]     | -                           | [0, 1, 2, 3, 4, 5]     | 
| 2         | 1  | 4       |   {100=0, 4=5, 200=2, 1=3, 3=4, 2=5} | Union(parent, 1, 4)            | [0, 4, 2, 3, 4, 5]     | -           |  [0, 4, 2, 3, 4, 5]    | 
| 3         | 2  | 200     |   {100=0, 4=5, 200=2, 1=3, 3=4, 2=5} | -                           | [0, 4, 2, 3, 4, 5]    | -                           | [0, 4, 2, 3, 4, 5]    | 
| 4         | 3  | 1       |   {100=0, 4=5, 200=2, 1=3, 3=4, 2=5} | -            | -    | Union(parent, 3, 5)            | [0, 4, 2, 5, 5, 5]     | 
| 5         | 4  | 3       |  {100=0, 4=5, 200=2, 1=3, 3=4, 2=5}  | Union(parent, 4, 5)            | [0, 4, 2, 5, 5, 5]     | Union(parent, 4, 1)            | [0, 5, 2, 5, 5, 5]     |
| 6         | 5  | 2       | {100=0, 4=5, 200=2, 1=3, 3=4, 2=5} | Union(parent, 5, 3)            | [0, 4, 2, 5, 5, 5]      |    Union(parent, 5, 4)                          | [0, 5, 2, 5, 5, 5]     | 

The final result is 3, and the longest consecutive sequence is [1, 2, 3, 4].




### In iteration 4:

In this we need to union element 3 and 2 and  also the element 3 and 4. 

so union(parent,4,5) and union(parent,4,1)  are called.  because   of the statement  `size.get(nums[i] + 1)`.

`size.get(2) =5` and `size.get(4)=1`

updated parent: `parent[ 0 4 2 3 4 5]`

###### in `union(parent,4,5)` the parents of the element is the element itself so it doesn't go inside the while loop and the updated array will be `[0 4 2 5 5 5]` . 

updated parent: `parent[ 0 4 2 5 4 5]`

######  now going to `union(parent,4,1)`. here, parent[1] is not equal to 1. so it goes into the while loop and updates parent[1]=parent[4] ` parent[1]=5` and c=4 

now the updated array is [0 5 2 5 5 5].  

now the while condition parent[4] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 3 4 5]`


### In iteration 5: (No changes)

In this we need to union element 3 and 2 and  also the element 3 and 4. 

######  so union(parent,5,3) and union(parent,5,4)  called.  because   of the statement  `size.get(nums[i] + 1)`.

`size.get(1) =3` and `size.get(3)=4`

updated parent: `parent[ 0 5 2 5 5 5]`


in `union(parent,5,3) ` here, parent[3] is not equal to 3. so it goes into the while loop and updates parent[3]=parent[5] ` parent[3]=5` and c=5 

now the while condition parent[5] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 5 5 5]`

######  now going to `union(parent,5,4) `. here, parent[4] is not equal to 4. so it goes into the while loop and updates parent[4]=parent[5] ` parent[4]=5` and c=5 

now the updated array is [0 5 2 5 5 5].  

now the while condition parent[5] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 5 5 5]`


### Calculating result value

1. `value` is the index of elements for the nums. Since in parents array the consecutive elements have the same value `parents=[0 5 2 5 5 5]` and `value=[ 0 1 2 3 4 5]` and `count[0 0 0 0 0]`
2. `findResult=find(parent,value)` finds the root node in the parents array.
3. `count[findValue]++` updates the index of the root value.
4. Then `res` value is compared and updated

   `parent =[0 5 2 5 5 5 5]`

   `nums=[100 4 200 1 3 2]`

   
| previous res|  value  |  findResult  | count   | res   |  
|-----------|----|---------|-----------------------------|-----------------------|
| 0|  0  |  0 find(parent,0) | [1 0 0 0 0 0 0]   | 1   |  
| 0|  1  |  5 find(parent,1) | [1 0 0 0 0 0 1]   | 1   | 
| 0|  2  |  2 find(parent,2) | [1 0 1 0 0 0 1]   | 1   |
| 0|  3  |  5 find(parent,3) | [1 0 1 0 0 0 2]   | 2   | 
| 0|  4  |  5  find(parent,4) | [1 0 1 0 0 0 3]   | 3  | 
| 0|  5  |  5 find(parent,5) | [1 0 1 0 0 0 4]   | 4   | 

Iterating over size.values():
inside while loop
Iteration0
parent[ 3] --> 3
find(parent, 5, 2) -> 5
count[find(parent, 3, 1)]++ -> count[5] = 1
Value: 3
Parent: 5
Count: [0, 0, 0, 0, 0, 1]
MaxCount: 1

find(parent, 5, 2) -> 5
count[find(parent, 5, 2)]++ -> count[5] = 2
Value: 5
Parent: 5
Count: [0, 0, 0, 0, 0, 2]
MaxCount: 2

inside while loop
Iteration0
parent[ 4] --> 4
find(parent, 5, 2) -> 5
count[find(parent, 4, 3)]++ -> count[5] = 3
Value: 4
Parent: 5
Count: [0, 0, 0, 0, 0, 3]
MaxCount: 3

find(parent, 0, 100) -> 0
count[find(parent, 0, 100)]++ -> count[0] = 1
Value: 0
Parent: 0
Count: [1, 0, 0, 0, 0, 3]
MaxCount: 3

inside while loop
Iteration0
parent[ 1] --> 1
find(parent, 5, 2) -> 5
count[find(parent, 1, 4)]++ -> count[5] = 4
Value: 1
Parent: 5
Count: [1, 0, 0, 0, 0, 4]
MaxCount: 4

find(parent, 2, 200) -> 2
count[find(parent, 2, 200)]++ -> count[2] = 1
Value: 2
Parent: 2
Count: [1, 0, 1, 0, 0, 4]
MaxCount: 4

Final Result: 4

### using sort function

``` java
 class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length==0){
            return 0;
        }
        int answer = 0;
        Arrays.sort(nums);
        int count=0;
        // 1,2,3,4,100,200
        for(int i=1 ;i<nums.length; i++){
            if(nums[i-1]==nums[i]){
                continue;
            }
            if(nums[i]-nums[i-1]==1){
                ++count;//count=3
            }else{
                answer = Math.max(answer, count);
                count=0;
            }
            answer = Math.max(answer, count);
        }
        return answer+1;
    }
}
```

### BFS 
``` java
  /*
     * BFS:
     *     ~ N + N => O(N) time,
     *     O(N) space
     */
    public int longestConsecutive2(int[] nums) {
        int maxLength = 0;
        if (nums.length != 0) {
            // get all numbers: ~ N => O(N) time
            HashMap<Integer, Integer> isVisited = new HashMap<>();
            for (int i = 0; i < nums.length; i++) {
                isVisited.put(nums[i], 0);
            }

            // BFS: ~ N => O(N) time
            for (int i = 0; i < nums.length; i++) {
                int length = 0;
                List<Integer> queue = new ArrayList<>(Arrays.asList(nums[i]));
                while (!queue.isEmpty()) {
                    int num = queue.remove(0),
                        prev = num - 1,
                        next = num + 1;
                    isVisited.put(num, 1); // mark as visited
                    length++;
                    if (isVisited.containsKey(prev) && isVisited.get(prev) == 0) {
                        isVisited.put(prev, 1); // mark as visited (avoid duplicate enqueue)
                        queue.add(prev);
                    }
                    if (isVisited.containsKey(next) && isVisited.get(next) == 0) {
                        isVisited.put(next, 1); // mark as visited (avoid duplicate enqueue)
                        queue.add(next);
                    }
                }
                if (maxLength < length) {
                    maxLength = length;
                }
            }
        }
        return maxLength;
    }
```

## UnorderedMap or HashMap method. Complexity O(n^2)

``` java 
    /*
     * hash map:
     */
    public int longestConsecutive(int[] nums) {
        int maxLength = 0;
        HashMap<Integer, Integer> lengthMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            if (!lengthMap.containsKey(num)) { // no sequence length
                // calculate sequence length (by iterating array so far)
                int prevLength = lengthMap.containsKey(num - 1)? lengthMap.get(num - 1): 0,
                    nextLength = lengthMap.containsKey(num + 1)? lengthMap.get(num + 1): 0,
                    length = 1 + prevLength + nextLength;

                // set the length value to the number
                lengthMap.put(num, length);

                // use prevLength and nextLength to locate the end numbers of the sequences to the left and right
                // set the length values to the end numbers (no impacts if the end number doesn't exist)
                lengthMap.put(num - prevLength, length);
                lengthMap.put(num + nextLength, length);

                if (maxLength < length) {
                    maxLength = length;
                }
            }
        }
        return maxLength;
    }
}
```



 
