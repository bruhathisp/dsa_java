## Solution



Lessons learnt:
1. Whenever there's a layer that grows over time use bfs or dfs
2. 
### Time Complexity 
The time complexity of the solution is O(rows * cols), where rows is the number of rows in the grid and cols is the number of columns in the grid.

Here's a breakdown of the time complexity:

**Grid Initialization:** O(rows * cols) - The algorithm initializes the grid and counts the number of fresh oranges, which requires iterating through the entire grid.

**BFS Iterations:** The main part of the algorithm involves using BFS to simulate the rotting process. In the worst case, each cell is added to the queue once and dequeued once. Therefore, the time complexity of BFS is O(rows * cols).

Total Time Complexity: O(rows * cols) + O(rows * cols) ≈ O(rows * cols)

The BFS iteration dominates the overall time complexity, and since each cell is processed at most once, the time complexity is proportional to the number of cells in the grid.

Breadth-First Search (BFS) is used in the "Rotting Oranges" problem because it involves exploring the grid in layers, level by level, starting from the initial rotten oranges. BFS is a natural fit for problems where you need to explore in a systematic way, layer by layer, especially when the problem involves finding the shortest path or the minimum number of steps.


**STEPS**
1. Count fresh oranges and add rotten oranges to the queue
2. use this array to simulate the 4 direction `int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};`
3. iterate till queue is empty, (i.e) till all possibilities of rotting oranges is iterated, and iterate till number of `freshOranges` > 0 that is till we still have fresh ones
4. open another iteration for the elements in the queue, eliminate the first element in the queue and spread the rot
5. how to jump directions? use this code `int newRow = current[0] + dir[0]; int newCol = current[1] + dir[1];`
6. How to rot oranges? use this code.     see if `newRow newCol` greater than 0 and lesser than rows and cols. and check if it is a freshorange. **DO NOT TURN 0 TO 2**
7. then add the newly rotten to the queue. 
   ``` java
             if (newRow >= 0 && newRow < rows && 
                        newCol >= 0 && newCol < cols && 
                        grid[newRow][newCol] == 1) {
                            
                        grid[newRow][newCol] = 2;
                        freshOranges--;
                        queue.add(new int[]{newRow, newCol});
   ```
8. atlast update the minutes before another layer begins


## Code 

``` java
          import java.util.LinkedList;
import java.util.Queue;


class Solution {
    public int orangesRotting(int[][] grid) {



       
        int rows = grid.length;
        int cols = grid[0].length;
        int freshOranges = 0;
        int minutes = 0;

        Queue<int[]> queue = new LinkedList<>();

        // Count fresh oranges and add rotten oranges to the queue
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 1) {
                    freshOranges++;
                } else if (grid[i][j] == 2) {
                    queue.add(new int[]{i, j});
                }
            }
        }

        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        while (!queue.isEmpty() && freshOranges > 0) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                int[] current = queue.poll();

                for (int[] dir : directions) {
                    int newRow = current[0] + dir[0];
                    int newCol = current[1] + dir[1];

                    if (newRow >= 0 && newRow < rows && 
                        newCol >= 0 && newCol < cols && 
                        grid[newRow][newCol] == 1) {
                            
                        grid[newRow][newCol] = 2;
                        freshOranges--;
                        queue.add(new int[]{newRow, newCol});
                    }
                }
            }

            if (!queue.isEmpty()) {
                minutes++;
            }
        }

        return freshOranges == 0 ? minutes : -1;
    }

        
    }
```
### Oytput

Initial Grid:
Fresh Oranges: 6
[0, 0] 
2 1 1 
1 1 0 
0 1 1 
---------------------
Grid after 1 minutes:
Fresh Oranges: 4
[1, 0] [0, 1] 
2 2 1 
2 1 0 
0 1 1 
---------------------
Grid after 2 minutes:
Fresh Oranges: 2
[1, 1] [0, 2] 
2 2 2 
2 2 0 
0 1 1 
---------------------
Grid after 3 minutes:
Fresh Oranges: 1
[2, 1] 
2 2 2 
2 2 0 
0 2 1 
---------------------
Grid after 4 minutes:
Fresh Oranges: 0
[2, 2] 
2 2 2 
2 2 0 
0 2 2 
---------------------
Minimum time to rot all oranges: 4
