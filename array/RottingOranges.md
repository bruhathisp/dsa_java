## Solution

Breadth-First Search (BFS) is used in the "Rotting Oranges" problem because it involves exploring the grid in layers, level by level, starting from the initial rotten oranges. BFS is a natural fit for problems where you need to explore in a systematic way, layer by layer, especially when the problem involves finding the shortest path or the minimum number of steps.

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
