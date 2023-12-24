## Solution


### What is backtracking
A brute force approach uses depth-first search(DFS) to explore all possible solutions. If the current solution is not satisfy the constraints, then eliminate that and go back(backtrack) and check for other solutions.
![image](https://github.com/bruhathisp/dsa_java/assets/91585301/9db3c0a9-e9c2-4992-8a9a-aacf4248ff05)
Source yuminlee2

I'll try to explain backtracking, The backtracking in this sum is used to find distinct combinations and avoids duplicate combinations. This diagram explains the process.

YOU CANNOT BRUTE FORCE WITH A DECISION TREE IF YOU WANT UNIQUE COMBINATIONS. WE END UP GETTING DUPLICATE COMBINATIONS.

It is a recursive method. Consider the array candidates[i] and target. We have a start variable to start over the iteration this is going to help us 

Step 1: Always target-candidates[i] is the next target. If the target is zero, we found the combination. 

Step 2: Else we go inside the loop. The value of i doesn't update till `target - candidates[i]` is true. Once it is false, the `backtracking` deletes the last element of the array.

It is like a stack LIFO. Now backtrack with start=i. and the whole process is done again.

For clarity [Watch the video](https://youtu.be/GBKI9VSKdGg?feature=shared)

Here's a step-by-step summary of the decision tree:

1. Start with an empty combination and a target of 7.

2. First decision: Include or skip the number 2.

3. If included, move to the next decision with a reduced target of 5.
If skipped, continue with the original target of 7.
Second decision: Include or skip another 2.

4. If included, move to the next decision with a further reduced target.
If skipped, continue the decision tree.
Third decision: Include or skip a third 2. Continue recursively.

5. If included, check if the target is met. If not, continue.
If skipped, move to the next decision.
Continue the process, making decisions for the number 3 and other available numbers.

6. When the target is met, add the current combination to the result.

7. If the target is exceeded or all elements are exhausted, backtrack to the previous decision point.

The recursive nature of the process ensures that different combinations are explored, and duplicates are avoided.

The key idea is to explore all possible combinations by making decisions at each step, considering the inclusion or exclusion of numbers. 

### Backtracking is employed to ensure unique combinations and avoid duplicates. 



Input: candidates = [2,3,6,7], target = 7

Output: [[2,2,3],[7]]

``` java

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        backtrack(candidates, target, 0, new ArrayList<>(), result);  //initial start=0
        
        return result;
    }

    private void backtrack(int[] candidates, int target, int start, List<Integer> currentSum, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(currentSum));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            if (target - candidates[i] >= 0) {
                currentSum.add(candidates[i]);  // insert value into array or push to the stack
                backtrack(candidates, target - candidates[i], i, currentSum, result);
                currentSum.remove(currentSum.size() - 1);     // delete the last element or pop out of the stack
            }
        }
    }
}





```
![image](https://github.com/bruhathisp/dsa_java/assets/91585301/ae1f6fb4-619d-45f2-89bd-bf65574fb378)

Source: yuminlee2



## Table for better understanding

This if for `i=0`. You can similarly see for all the i values ranging from `0 to candidates.length`

Let's represent the progress in a table:

| Iteration | Start | Current Combination | Target | Action                  | Result                                    |
|-----------|-------|----------------------|--------|-------------------------|-------------------------------------------|
| 0         | 0     | []                   | 7      | -                       | -                                         |
| 1         | 0     | [2]                  | 5      | Include 2 at index 0    | -                                         |
| 2         | 0     | [2, 2]               | 3      | Include 2 at index 0    | -                                         |
| 3         | 0     | [2, 2, 2]            | 1      | Include 2 at index 0    | -                                         |
| 4         | 0     | [2, 2, 2, 2]         | -1     | Exclude 2 at index 0    | -                                         |
| 5         | 1     | [2, 2, 2, 3]         | 0      | Combination found       | [[2, 2, 3]]                               |
| 6         | 1     | [2, 2, 3]            | 1      | Exclude 3 at index 1    | -                                         |
| 7         | 2     | [2, 3]               | 4      | Include 3 at index 1    | -                                         |
| 8         | 2     | [2, 3, 3]            | 1      | Include 3 at index 1    | -                                         |
| 9         | 2     | [2, 3, 3, 6]         | -5     | Exclude 6 at index 2    | -                                         |
| 10        | 3     | [2, 3, 6]            | 1      | Exclude 6 at index 2    | -                                         |
| 11        | 3     | [2, 7]               | -1     | Exclude 7 at index 3    | -                                         |
| 12        | 4     | [3]                  | 4      | Include 3 at index 1    | -                                         |
| 13        | 4     | [3, 3]               | 1      | Include 3 at index 1    | -                                         |
| 14        | 4     | [3, 3, 6]            | -2     | Exclude 6 at index 2    | -                                         |
| 15        | 5     | [3, 6]               | 1      | Exclude 6 at index 2    | -                                         |
| 16        | 5     | [7]                  | -1     | Combination found       | [[2, 2, 3], [7]]                         |

Now, the [7] combination is included in the table.
This table shows each iteration of the algorithm, indicating the values of `i`, `start`, `currentSum`, the action taken, and the resulting combinations.
