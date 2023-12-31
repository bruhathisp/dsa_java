## Solution

1. After calling `borderFrom` function decrement both m(row) and n(col) by 2
2. `borderFrom` functions takes 4 turns each call. and this is how it adds to the `res` array.
   ``` java
    for (int i = c; i <= c + n - 1; i++) { // Horizontal Top
            res.add(matrix[r][i]);   
        }
        for (int i = r + 1; i <= r + m - 2; i++) {  //Vertical down, start from second row coz the first row is already done
            res.add(matrix[i][c + n - 1]);
        }
        if (m > 1) {
            for (int i = c + n - 1; i >= c; i--) { // Vertical up, start from the last-1 is already done last column
                res.add(matrix[r + m - 1][i]);
            }
        }
        if (n > 1) {  //horizontal right start from r+m -2 
            for (int i = r + m - 2; i >= r + 1; i--) {
                res.add(matrix[i][c]);
            }
        }
    ```
![image](https://github.com/bruhathisp/dsa_java/assets/91585301/12d4d015-5729-4ffc-817a-d86be9cdaff9)

| Iteration | p | m | n | i | c | r | Values Added      | res                                          |
|-----------|---|---|---|---|---|---|-------------------|----------------------------------------------|
| 1         | 0 | 3 | 4 | 0 | 0 | 0 | [1]               | [1]                                          |
|           |   |   |   | 1 | 0 | 0 | [1, 2]            | [1, 2]                                       |
|           |   |   |   | 2 | 0 | 0 | [1, 2, 3]         | [1, 2, 3]                                    |
|           |   |   |   | 3 | 0 | 0 | [1, 2, 3, 4]      | [1, 2, 3, 4]                                 |
|           |   |   |   | 1 | 0 | 0 | [1, 2, 3, 4, 8]   | [1, 2, 3, 4, 8]                              |
|           |   |   |   |   |   |   |                   | res: [1, 2, 3, 4, 8]                         |
|           |   |   |   | 3 | 0 | 0 | [1, 2, 3, 4, 8, 12]| [1, 2, 3, 4, 8, 12]                          |
|           |   |   |   | 2 | 0 | 0 | [1, 2, 3, 4, 8, 12, 11]| [1, 2, 3, 4, 8, 12, 11]                   |
|           |   |   |   | 1 | 0 | 0 | [1, 2, 3, 4, 8, 12, 11, 10]| [1, 2, 3, 4, 8, 12, 11, 10]            |
|           |   |   |   | 0 | 0 | 0 | [1, 2, 3, 4, 8, 12, 11, 10, 9]| [1, 2, 3, 4, 8, 12, 11, 10, 9]     |
|           |   |   |   | 1 | 0 | 0 | [1, 2, 3, 4, 8, 12, 11, 10, 9, 5]| [1, 2, 3, 4, 8, 12, 11, 10, 9, 5]|
|           |   |   |   |   |   |   |                   | res: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5]    |
| 2         | 1 | 1 | 2 | 1 | 1 | 1 | [6, 7]            | [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7] |
|           |   |   |   | 2 | 1 | 1 |                   | res: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]|


``` java
    class Solution {
    List<Integer> res = new ArrayList<>();
    int[][] matrix;
    int m;
    int n;
    
    private void borderFrom(int r, int c) {
        for (int i = c; i <= c + n - 1; i++) {
            res.add(matrix[r][i]);
        }
        for (int i = r + 1; i <= r + m - 2; i++) {
            res.add(matrix[i][c + n - 1]);
        }
        if (m > 1) {
            for (int i = c + n - 1; i >= c; i--) {
                res.add(matrix[r + m - 1][i]);
            }
        }
        if (n > 1) {
            for (int i = r + m - 2; i >= r + 1; i--) {
                res.add(matrix[i][c]);
            }
        }
    }

    public List<Integer> spiralOrder(int[][] matrix) {
        this.matrix = matrix;
        m = matrix.length;
        n = matrix[0].length;
        int p = 0;
        while (n > 0 && m > 0) {
            borderFrom(p, p);
            p++;
            m -= 2;
            n -= 2;
        }
        return res;
    }
}

```


Tescase 1

Input [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

Output [1, 2, 3, 6, 9, 8, 7, 4, 5]

Tescase 1

Input [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]

Output [1, 2, 3, 6, 9, 8, 7, 4, 5, 1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]



### Explanation 

[[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]

p: 0, m: 3, n: 4
i: 0, c: 0, r: 0, m: 3, n: 4
[1]

i: 1, c: 0, r: 0, m: 3, n: 4
[1, 2]

i: 2, c: 0, r: 0, m: 3, n: 4
[1, 2, 3]

i: 3, c: 0, r: 0, m: 3, n: 4
[1, 2, 3, 4]

i: 1, c: 3, r: 0, m: 3, n: 4
[1, 2, 3, 4, 8]

i: 3, c: 0, r: 2, m: 3, n: 4
[1, 2, 3, 4, 8, 12]

i: 2, c: 0, r: 2, m: 3, n: 4
[1, 2, 3, 4, 8, 12, 11]

i: 1, c: 0, r: 2, m: 3, n: 4
[1, 2, 3, 4, 8, 12, 11, 10]

i: 0, c: 0, r: 2, m: 3, n: 4
[1, 2, 3, 4, 8, 12, 11, 10, 9]

i: 1, c: 0, r: 1, m: 3, n: 4
[1, 2, 3, 4, 8, 12, 11, 10, 9, 5]

res: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5]

p: 1, m: 1, n: 2

i: 1, c: 1, r: 1, m: 1, n: 2
[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6]

i: 2, c: 1, r: 1, m: 1, n: 2
[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]

res: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]

[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]



#Code to generate print statements

``` java
      import java.util.*;

public class MyClass {
    List<Integer> res = new ArrayList<>();
    int[][] matrix;
    int m;
    int n;

    private void borderFrom(int r, int c) {
        for (int i = c; i <= c + n - 1; i++) {
        System.out.println("i: " + i + ", c: " + c + ", r: " + r + ", m: " + m + ", n: " + n);
        res.add(matrix[r][i]);
        System.out.println(res);
    }
    for (int i = r + 1; i <= r + m - 2; i++) {
        System.out.println("i: " + i + ", c: " + (c + n - 1) + ", r: " + r + ", m: " + m + ", n: " + n);
        res.add(matrix[i][c + n - 1]);
        System.out.println(res);
    }
    if (m > 1) {
        for (int i = c + n - 1; i >= c; i--) {
            System.out.println("i: " + i + ", c: " + c + ", r: " + (r + m - 1) + ", m: " + m + ", n: " + n);
            res.add(matrix[r + m - 1][i]);
            System.out.println(res);
        }
    }
    if (n > 1) {
        for (int i = r + m - 2; i >= r + 1; i--) {
            System.out.println("i: " + i + ", c: " + c + ", r: " + i + ", m: " + m + ", n: " + n);
            res.add(matrix[i][c]);
            System.out.println(res);
        }
    }
    }

    public List<Integer> spiralOrder(int[][] matrix) {
        this.matrix = matrix;
        m = matrix.length;
        n = matrix[0].length;
        int p = 0;
        while (n > 0 && m > 0) {
            System.out.println("p: " + p + ", m: " + m + ", n: " + n);
            borderFrom(p, p);
            System.out.println("res: " + res);
            p++;
            m -= 2;
            n -= 2;
        }
        return res;
    }

    public static void main(String[] args) {
        MyClass myClass = new MyClass();

        // int[][] matrix1 = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        // System.out.println(Arrays.deepToString(matrix1));
        // System.out.println(myClass.spiralOrder(matrix1));

        int[][] matrix2 = {{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}};
        System.out.println(Arrays.deepToString(matrix2));
        System.out.println(myClass.spiralOrder(matrix2));
    }
}

```



