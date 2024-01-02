## Solution

This code appears to be an implementation of a shortest path algorithm for a binary matrix, using a breadth-first search (BFS) approach. Let's break down the key concepts:

### 1. Intuition:
- The problem involves finding the shortest path in a binary matrix, where 0 represents an open cell and 1 represents an obstacle.
- The observation is that adjacent vertices follow a non-linear graph topology.

### 2. DFS or BFS?
- The order of traversal needs to be in a regular fashion, either depth-first or breadth-first.
- The decision is made to use BFS because the problem requires finding the shortest path, and BFS explores nodes level by level.

### 5. Use BFS over DFS because we need the shortest path.


### 4. Approach:
- The chosen approach is to perform amortized BFS on all possible adjacent vertices.
- A visited array is maintained to mark vertices that have already been seen.

### 5. Why Maintain a Visited Array?
- Once a vertex is seen, it is considered to be already seen following the shortest path.
- Therefore, the algorithm marks it as visited as soon as it is seen.

### 6. Complexity:
- Time Complexity: O(n^2) where 'n' is the size of the matrix.
- Space Complexity: O(n^2) for the visited array.

### 7. Code:
- It is a square natrix of N x N.
- The code defines a `Vertex` class representing a cell with its row, column, and length so far. `r,c,l`
- The `isValid` function checks whether a given cell is within bounds, is a valid move `grid[row][col] == 0`, and is unvisited `isSeen[row][col] == false) `
- The main BFS loop explores neighboring cells and updates the queue.
- Steps:
1. Make a boolean array `isSeen` of dimension N x N. and Queue<Vertex> `adjacentCells`. So adjacent cell is a Queue of Vertex(r,c,l). Also initate `len`=-1.
2. Start with the index (0,0) and add it into the `adjacentCells` if it is valid.
3. Now the iteration begins and it executes till `adjacentCells` is empty.
4. Take the `currentCell`  as `Vertex currCell = adjacentCells.remove();` It is a queue so LIFO.
5. Calculate for Vertex downright, downleft, upright, upleft, up, left, right, down. For example, `if (isValid(row + 1, col - 1, N, grid, isSeen))` this is for downleft.
6. The algorithm terminates when reaching the bottom-right corner of the matrix or when there are no more cells to explore.
 ``` java
   if (row == N - 1 && col == N - 1)
            {
                len = currCell.lenSoFar;
                break;
            }
   ```
7. Return -1 or currentLen, whatever the process is.
   





``` java
  class Solution {
    
    class Vertex {
        int row;
        int col;
        int lenSoFar;

        Vertex(int row, int col, int lenSoFar) {
            this.row = row;
            this.col = col;
            this.lenSoFar = lenSoFar;
        }
    }

    public boolean isValid(int row, int col, int N, int [][]grid, boolean [][]isSeen) {
        if ( row >= 0 && row < N 
        && col >= 0 && col < N
         && grid[row][col] == 0
          && isSeen[row][col] == false) {
              isSeen[row][col] = true;
              return true;
          }
        return false;
    }

   
    public int shortestPathBinaryMatrix(int[][] grid) {
        
        int N = grid[0].length;

        // printGrid(grid, N);

        int len = -1;

        boolean[][] isSeen = new boolean[N][N];
        Queue<Vertex> adjacentCells = new LinkedList<>();

        if (isValid(0, 0, N, grid, isSeen))
        {
            Vertex root = new Vertex(0, 0, 1);
            adjacentCells.add(root);
        }

        while (adjacentCells.isEmpty() == false) {

            Vertex currCell = adjacentCells.remove();
            int row = currCell.row, col = currCell.col;

            

            if (row == N - 1 && col == N - 1)
            {
                len = currCell.lenSoFar;
                break;
            }

            // Vertex downright, downleft, upright, upleft, up, left, right, down;
            if (isValid(row + 1, col + 1, N, grid, isSeen))
            {
                Vertex downright = new Vertex(row + 1, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(downright);
            }

            if (isValid(row + 1, col - 1, N, grid, isSeen))
            {
                Vertex downleft = new Vertex(row + 1, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(downleft);
            }

            if (isValid(row - 1, col + 1, N, grid, isSeen))
            {
                Vertex upright = new Vertex(row - 1, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(upright);
            }

            if (isValid(row - 1, col - 1, N, grid, isSeen))
            {
                Vertex upleft = new Vertex(row - 1, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(upleft);
            }

            if (isValid(row - 1, col, N, grid, isSeen))
            {
                Vertex up = new Vertex(row - 1, col, currCell.lenSoFar + 1);
                adjacentCells.add(up);
            }

            if (isValid(row, col - 1, N, grid, isSeen))
            {
                Vertex left = new Vertex(row, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(left);
            }

            if (isValid(row + 1, col, N, grid, isSeen))
            {
                Vertex down = new Vertex(row + 1, col, currCell.lenSoFar + 1);
                adjacentCells.add(down);
            }

            if (isValid(row, col + 1, N, grid, isSeen))
            {
                Vertex right = new Vertex(row, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(right);
            }
        }
        return len;
    }
}

```


## Output 

Let's break down the provided table and the output of the code for each stage:

**Input Matrix:**
```plaintext
A = [[1, 2, 3, 4], 
     [2, 2, 3, 4], 
     [3, 2, 3, 4], 
     [4, 5, 6, 7]]
```

**Dynamic Programming Table (dp) Initialization:**
```plaintext
dp = [[1, 0, 0, 0], 
      [0, 0, 0, 0], 
      [0, 0, 0, 0], 
      [0, 0, 0, 0]]
```

**First Loop (Rows) - i: 1 to 3:**
```plaintext
i: 1, j: 0, A[i - 1][0] < A[i][0]: 1 < 2, dp[1][0]: 2, res: 2
dp = [[1, 0, 0, 0], 
      [2, 0, 0, 0], 
      [0, 0, 0, 0], 
      [0, 0, 0, 0]]

i: 2, j: 0, A[i - 1][0] < A[i][0]: 2 < 3, dp[2][0]: 3, res: 3
dp = [[1, 0, 0, 0], 
      [2, 0, 0, 0], 
      [3, 0, 0, 0], 
      [0, 0, 0, 0]]

i: 3, j: 0, A[i - 1][0] < A[i][0]: 3 < 4, dp[3][0]: 4, res: 4
dp = [[1, 0, 0, 0], 
      [2, 0, 0, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]
```

**Second Loop (Columns) - i: 1 to 2:**
```plaintext
i: 0, j: 1, A[0][i - 1] < A[0][i]: 1 < 2, dp[0][i]: 2, res: 4
dp = [[1, 2, 0, 0], 
      [2, 0, 0, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]

i: 0, j: 2, A[0][i - 1] < A[0][i]: 2 < 3, dp[0][i]: 3, res: 4
dp = [[1, 2, 3, 0], 
      [2, 0, 0, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]

i: 0, j: 3, A[0][i - 1] < A[0][i]: 3 < 4, dp[0][i]: 4, res: 4
dp = [[1, 2, 3, 4], 
      [2, 0, 0, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]
```

**Third Loop (Nested Rows and Columns) - i: 1 to 3, j: 1 to 3:**
```plaintext
No conditions met at i: 1, j: 1, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, 0, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]

No conditions met at i: 1, j: 2, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, -1, 0], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]

No conditions met at i: 1, j: 3, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, 0, 0, 0], 
      [4, 0, 0, 0]]

No conditions met at i: 2, j: 1, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, 0, 0], 
      [4, 0, 0, 0]]

No conditions met at i: 2, j: 2, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, -1, 0], 
      [4, 0, 0, 0]]

No conditions met at i: 2, j: 3, dp[i][j]: -1
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, -1, -1], 
      [4, 0, 0, 0]]

i: 3, j: 1, A[i][j] > A[i][j - 1]: 5 > 4, dp[i][j]: 0, res: 4
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, -1, -1], 
      [4, 5, 0, 0]]

i: 3, j: 2, A[i][j] > A[i][j - 1]: 6 > 5, dp[i][j]: 0, res: 5
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, -1, -1], 
      [4, 5, 6, 0]]

i: 3, j: 3, A[i][j] > A[i][j - 1]: 7 > 6, dp[i][j]: 

0, res: 6
dp = [[1, 2, 3, 4], 
      [2, -1, -1, -1], 
      [3, -1, -1, -1], 
      [4, 5, 6, 7]]
```

**Result:**
```plaintext
The length of the longest increasing path ending at the bottom-right corner: 7
```

This shows how the dynamic programming table is updated in each iteration of the loops and how the algorithm finds the length of the longest increasing path. The final result is 7, indicating the length of the longest increasing path from the top-left to the bottom-right corner.

### To make the above print use this code 

``` java

     class Solution {
    
    class Vertex {
        int row;
        int col;
        int lenSoFar;

        Vertex(int row, int col, int lenSoFar) {
            this.row = row;
            this.col = col;
            this.lenSoFar = lenSoFar;
        }
    }

    public boolean isValid(int row, int col, int N, int [][]grid, boolean [][]isSeen) {
        if ( row >= 0 && row < N 
        && col >= 0 && col < N
         && grid[row][col] == 0
          && isSeen[row][col] == false) {
              isSeen[row][col] = true;
              return true;
          }
        return false;
    }

   
    public int shortestPathBinaryMatrix(int[][] grid) {
        
        int N = grid[0].length;

        // printGrid(grid, N);

        int len = -1;

        boolean[][] isSeen = new boolean[N][N];
        Queue<Vertex> adjacentCells = new LinkedList<>();

        if (isValid(0, 0, N, grid, isSeen))
        {
            Vertex root = new Vertex(0, 0, 1);
            adjacentCells.add(root);
        }

        while (adjacentCells.isEmpty() == false) {

            Vertex currCell = adjacentCells.remove();
            int row = currCell.row, col = currCell.col;

            

            if (row == N - 1 && col == N - 1)
            {
                len = currCell.lenSoFar;
                break;
            }

            // Vertex downright, downleft, upright, upleft, up, left, right, down;
            if (isValid(row + 1, col + 1, N, grid, isSeen))
            {
                Vertex downright = new Vertex(row + 1, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(downright);
            }

            if (isValid(row + 1, col - 1, N, grid, isSeen))
            {
                Vertex downleft = new Vertex(row + 1, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(downleft);
            }

            if (isValid(row - 1, col + 1, N, grid, isSeen))
            {
                Vertex upright = new Vertex(row - 1, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(upright);
            }

            if (isValid(row - 1, col - 1, N, grid, isSeen))
            {
                Vertex upleft = new Vertex(row - 1, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(upleft);
            }

            if (isValid(row - 1, col, N, grid, isSeen))
            {
                Vertex up = new Vertex(row - 1, col, currCell.lenSoFar + 1);
                adjacentCells.add(up);
            }

            if (isValid(row, col - 1, N, grid, isSeen))
            {
                Vertex left = new Vertex(row, col - 1, currCell.lenSoFar + 1);
                adjacentCells.add(left);
            }

            if (isValid(row + 1, col, N, grid, isSeen))
            {
                Vertex down = new Vertex(row + 1, col, currCell.lenSoFar + 1);
                adjacentCells.add(down);
            }

            if (isValid(row, col + 1, N, grid, isSeen))
            {
                Vertex right = new Vertex(row, col + 1, currCell.lenSoFar + 1);
                adjacentCells.add(right);
            }
        }
        return len;
    }
}

```

