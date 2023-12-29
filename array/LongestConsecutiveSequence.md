
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
            if(size.containsKey(nums[i] - 1) && find(parent,  i) != find(parent,size.get(nums[i] - 1))){
                union(parent, i, size.get(nums[i] - 1));
            }
            if(size.containsKey(nums[i] + 1) && find(parent, i) != find(parent, size.get(nums[i] + 1))){
                union(parent, i, size.get(nums[i] + 1));
            }
        }
        //return max 
        int res = 0;
        int[] count = new int[parent.length];
        for(int value : size.values()){
            count[find(parent, value)] ++;
            res = Math.max(res, count[find(parent, value)]);
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

Let's go through the code step by step and create a table to keep track of the parameters and function calls. The input array is [100, 4, 200, 1, 3, 2], and the expected output is 4.

| Iteration | i  | nums[i] |  size | Union Operation (nums[i] - 1) | Updated Parent Array | Union Operation (nums[i] + 1) | Updated Parent Array | 
|-----------|----|---------|-----------------------------|-----------------------|------|-----------------------------|-----------------------|
| 1         | 0  | 100     |  {100=0} | -                           | [0, 1, 2, 3, 4, 5]     | -                           | [0, 1, 2, 3, 4, 5]     | 
| 2         | 1  | 4       |  {100=0, 4=1} | Union(parent, 1, 4)            | [0, 4, 2, 3, 4, 5]     | -           |  [0, 4, 2, 3, 4, 5]    | 
| 3         | 2  | 200     |  {100=0, 4=4, 200=2} | -                           | [0, 4, 2, 3, 4, 5]    | -                           | [0, 4, 2, 3, 4, 5]    | 
| 4         | 3  | 1       |  {100=0, 4=4, 200=2, 1=3} | -            | -    | Union(parent, 3, 5)            | [0, 4, 2, 5, 5, 5]     | 
| 5         | 4  | 3       |  {100=0, 4=4, 200=2, 1=5, 3=4} | Union(parent, 4, 5)            | [0, 4, 2, 5, 5, 5]     | Union(parent, 4, 1)            | [0, 5, 2, 5, 5, 5]     |
| 6         | 5  | 2       | {100=0, 4=5, 200=2, 1=5, 3=5, 2=5} | Union(parent, 5, 3)            | [0, 4, 2, 5, 5, 5]      |    Union(parent, 5, 4)                          | [0, 5, 2, 5, 5, 5]     | 

The final result is 3, and the longest consecutive sequence is [1, 2, 3, 4].




In iteration 4:

In this we need to union element 3 and 2 and  also the element 3 and 4. 

so union(parent,4,5) and union(parent,4,1)  are called.  because   of the statement  `size.get(nums[i] + 1)`.

`size.get(2) =5` and `size.get(4)=1`

updated parent: `parent[ 0 4 2 3 4 5]`

in `union(parent,4,5)` the parents of the element is the element itself so it doesn't go inside the while loop and the updated array will be `[0 4 2 5 5 5]` . 

updated parent: `parent[ 0 4 2 5 4 5]`

now going to `union(parent,4,1)`. here, parent[1] is not equal to 1. so it goes into the while loop and updates parent[1]=parent[4] ` parent[1]=5` and c=4 

now the updated array is [0 5 2 5 5 5].  

now the while condition parent[4] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 3 4 5]`


In iteration 5: (No changes)

In this we need to union element 3 and 2 and  also the element 3 and 4. 

so union(parent,5,3) and union(parent,5,4)  called.  because   of the statement  `size.get(nums[i] + 1)`.

`size.get(1) =3` and `size.get(3)=4`

updated parent: `parent[ 0 5 2 5 5 5]`


in `union(parent,5,3) ` here, parent[3] is not equal to 3. so it goes into the while loop and updates parent[3]=parent[5] ` parent[3]=5` and c=5 

now the while condition parent[5] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 5 5 5]`

now going to `union(parent,5,4) `. here, parent[4] is not equal to 4. so it goes into the while loop and updates parent[4]=parent[5] ` parent[4]=5` and c=5 

now the updated array is [0 5 2 5 5 5].  

now the while condition parent[5] != 5 which is not true so the loop breaks.

updated parent: `parent[ 0 5 2 5 5 5]`


 
