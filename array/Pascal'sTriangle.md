## Solution

[Watch this video](https://youtu.be/FUkCOa4nkQQ?feature=shared)



Pascal's triangle, each number is the sum of the two numbers directly above it.
![image](https://github.com/bruhathisp/dsa_java/assets/91585301/2c7b2a27-1bb3-4007-9508-38fabe0d4080)

We know that i is rows and j is columns and here i=j.
1. `j == 0 || j == i`  add 1 in the `row` initial value
2. `previousRow = pascal.get(i - 1);` get the previous row.
3. now the pascal triangle is calculated by adding the above element and its left element in `previousRow` to `row`
4. after j is ended add row to `pascal`
5. after i ends return `pascal`

   

## Code 

``` java
     import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> pascal = new ArrayList<>();

        for (int i = 0; i < numRows; i++) {
            List<Integer> row = new ArrayList<>();

            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i) {
                    row.add(1);
                } else {
                    List<Integer> previousRow = pascal.get(i - 1);
                    row.add(previousRow.get(j - 1) + previousRow.get(j));
                }
            }

            pascal.add(row);
        }

        return pascal;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        List<List<Integer>> result = solution.generate(5);

        // Print the result
        for (List<Integer> row : result) {
            System.out.println(row);
        }
    }
}


```
