## Solution

### Why  Middle method ?

In the provided code, the division of functionality into three methods (`sudokuSolver`, `solveSudoku`, and `dfs`) serves to improve code readability, modularity, and separation of concerns. Let's discuss the roles of each method:

1. **`sudokuSolver` Method:**
   - This method serves as the entry point for solving the Sudoku puzzle.
   - It takes the initial Sudoku board (`sudoku`) as an input parameter and calls the `solveSudoku` method to start the solving process.
   - By having a high-level method like this, the main goal of solving the Sudoku puzzle is clear and encapsulated.



2. **`solveSudoku` Method:**
   - This method is responsible for initializing the solving process by calling the `dfs` method with the initial state of the Sudoku board and the starting position (0).
   - It acts as a bridge between the high-level `sudokuSolver` method and the low-level `dfs` method, encapsulating the details of the solving algorithm.


3. **`dfs` Method:**
   - This method implements the depth-first search (DFS) algorithm for solving the Sudoku puzzle.
   - It recursively explores different possibilities by trying to fill each cell with numbers ('1' to '9') and backtracking when necessary.
   - The boolean return type indicates whether a valid solution is found (`true`) or not (`false`).
   - Separating the DFS logic into its own method allows for better organization and makes the code more modular.

```java
private boolean dfs(char[][] board, int s) {
    if (s == 81)
        return true;
    // ... (rest of the DFS logic)
}
```

By breaking down the solving process into these methods, each method has a clear responsibility, making the code easier to understand, maintain, and modify. It adheres to the principle of separation of concerns, where each method focuses on a specific aspect of the overall task. This structure also facilitates testing and debugging of individual components.












### dfs 

This part of the code is crucial for the depth-first search (DFS) algorithm used to solve the Sudoku puzzle. Let's break it down:


1. **Checking if the Puzzle is Solved (`if (s == 81) return true;`):**
   - If `s` reaches 81, it means all cells in the Sudoku board have been filled, and the puzzle is solved. In this case, the method returns `true`, indicating a successful solution.

2. **Calculating Row and Column Indices (`final int i = s / 9; final int j = s % 9;`):**
   - `i` and `j` are calculated based on the current position `s` in the Sudoku board. `i` represents the row index, and `j` represents the column index.

3. **Checking if the Current Cell is Filled (`if (board[i][j] != '.') return dfs(board, s + 1);`):**
   - If the current cell at position (`i`, `j`) is already filled (not equal to '.'), it means it has a predefined value in the original puzzle. In this case, the algorithm moves to the next cell by calling `dfs(board, s + 1)` recursively.

4. **Trying Different Numbers (`for (char c = '1'; c <= '9'; ++c) ...`):**
   - If the current cell is empty, the algorithm enters a loop to try placing numbers ('1' to '9') in the cell.
   - It checks the validity of placing each number using the `isValid` method.
   - If placing a number is valid, it updates the board, calls `dfs` recursively for the next cell (`s + 1`), and checks if it leads to a successful solution (`if (dfs(board, s + 1)) return true;`).
   - If a valid placement is found, the method returns `true`, indicating a successful solution for the current cell.
   - If no valid number is found for the current cell, it backtracks by resetting the cell to an empty state (`board[i][j] = '.'`) and continues the loop.

5. **Returning `false` if No Valid Number is Found:**
   - If the loop exhausts all possibilities and no valid number is found for the current cell, the method returns `false`. This triggers backtracking to the previous cell in the recursive stack.

This combination of recursion, backtracking, and checking for a completed puzzle makes the algorithm explore different possibilities until a valid solution is found or all possibilities are exhausted.

### Is valid?

This code is part of the `isValid` method, which checks whether it's valid to place a given character `c` at a specific position (`row`, `col`) on the Sudoku board. Let's break down the logic:


1. **Checking Rows (`board[i][col] == c`):**
   - `board[i][col] == c` checks if the character `c` is already present in the same column (`col`) for any row (`i`). If it is, the placement is not valid, and `false` is returned.

2. **Checking Columns (`board[row][i] == c`):**
   - `board[row][i] == c` checks if the character `c` is already present in the same row (`row`) for any column (`i`). If it is, the placement is not valid, and `false` is returned.

3. **Checking 3x3 Subgrids (`board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c`):**
   - This part checks if the character `c` is already present in the 3x3 subgrid containing the cell at position (`row`, `col`).
   - `3 * (row / 3) + i / 3` calculates the starting row index of the current 3x3 subgrid.
   - `3 * (col / 3) + i % 3` calculates the starting column index of the current 3x3 subgrid.
   - This effectively checks if the character `c` is already present in the 3x3 subgrid covering the current cell (`row`, `col`).
   - If `c` is found in the subgrid, the placement is not valid, and `false` is returned.

If none of the above conditions is met for any `i` (from 0 to 8), it means the placement of `c` in the current position (`row`, `col`) is valid, and `true` is returned.




``` java
   class Solution {
    void sudokuSolver(char[][] sudoku) {
        solveSudoku(sudoku);
    }

    private void solveSudoku(char[][] board) {
        dfs(board, 0);
    }

    private boolean dfs(char[][] board, int s) {
        if (s == 81)
            return true;

        final int i = s / 9;
        final int j = s % 9;

        if (board[i][j] != '.')
            return dfs(board, s + 1);

        for (char c = '1'; c <= '9'; ++c)
            if (isValid(board, i, j, c)) {
                board[i][j] = c;
                if (dfs(board, s + 1))
                    return true;
                board[i][j] = '.';
            }

        return false;
    }

    private boolean isValid(char[][] board, int row, int col, char c) {
        for (int i = 0; i < 9; ++i)
            if (board[i][col] == c || board[row][i] == c ||
                board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c)
                return false;
        return true;
    }
}


```


```
i: 0, j: 0
Current Board:
2 5 . . . 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 0
isValid Status: false
Cell already filled. Moving to the next.
i: 0, j: 1
Current Board:
2 5 . . . 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 1
isValid Status: false
Cell already filled. Moving to the next.
i: 0, j: 2
Current Board:
2 5 . . . 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 2
isValid Status: false
Trying to place '1' at (0, 2)
i: 0, j: 3
Current Board:
2 5 1 . . 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 3
isValid Status: false
Trying to place '4' at (0, 3)
i: 0, j: 4
Current Board:
2 5 1 4 . 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 4
isValid Status: false
Trying to place '7' at (0, 4)
i: 0, j: 5
Current Board:
2 5 1 4 7 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 5
isValid Status: false
Cell already filled. Moving to the next.
i: 0, j: 6
Current Board:
2 5 1 4 7 3 . . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 6
isValid Status: false
Trying to place '6' at (0, 6)
i: 0, j: 7
Current Board:
2 5 1 4 7 3 6 . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . -7 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 7
isValid Status: false
Trying to place '8' at (0, 7)
i: 0, j: 8
Current Board:
2 5 1 4 7 3 6 8 . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 8
isValid Status: false
No valid number to place. Backtracking from (0, 8)
Backtracking from (0, 7)
No valid number to place. Backtracking from (0, 7)
Backtracking from (0, 6)
Trying to place '9' at (0, 6)
i: 0, j: 7
Current Board:
2 5 1 4 7 3 9 . . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 7
isValid Status: false
Trying to place '6' at (0, 7)
i: 0, j: 8
Current Board:
2 5 1 4 7 3 9 6 . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 8
isValid Status: false
No valid number to place. Backtracking from (0, 8)
Backtracking from (0, 7)
Trying to place '8' at (0, 7)
i: 0, j: 8
Current Board:
2 5 1 4 7 3 9 8 . 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 8
isValid Status: false
Trying to place '6' at (0, 8)
i: 1, j: 0
Current Board:
2 5 1 4 7 3 9 8 6 
. . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 9
isValid Status: false
Trying to place '3' at (1, 0)
i: 1, j: 1
Current Board:
2 5 1 4 7 3 9 8 6 
3 . . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 10
isValid Status: false
Trying to place '4' at (1, 1)
i: 1, j: 2
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 . . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 11
isValid Status: false
Trying to place '6' at (1, 2)
i: 1, j: 3
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 . . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 12
isValid Status: false
Trying to place '5' at (1, 3)
i: 1, j: 4
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 5 . . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 13
isValid Status: false
Trying to place '1' at (1, 4)
i: 1, j: 5
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 5 1 . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 14
isValid Status: false
No valid number to place. Backtracking from (1, 5)
Backtracking from (1, 4)
Trying to place '8' at (1, 4)
i: 1, j: 5
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 5 8 . 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 14
isValid Status: false
Trying to place '1' at (1, 5)
i: 1, j: 6
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 5 8 1 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 15
isValid Status: false
Cell already filled. Moving to the next.
i: 1, j: 7
Current Board:
2 5 1 4 7 3 9 8 6 
3 4 6 5 8 1 2 7 . 
8 7 . . . 6 4 . . 
. 2 . . . 8 1 9 3 
. 1 5 . 4 . 8 . . 
. . . 1 . . . . 4 
. . . 7 3 4 . . . 
. . . 6 . . . . 9 
. 6 4 . . 9 . 5 8 
s: 16
isValid Status: false
Cell already filled. Moving to the next.
i: 1, j: 8

```


``` java
   public class MyClass {
   static void sudokuSolver(char[][] sudoku) {
        solveSudoku(sudoku);
    }

    private static  void solveSudoku(char[][] board) {
        dfs(board, 0);
    }

    private static  boolean dfs(char[][] board, int s) {
        if (s == 81)
            return true;

        final int i = s / 9;
        final int j = s % 9;

        System.out.println("i: " + i + ", j: " + j);
        System.out.println("Current Board:");
        printBoard(board);
        System.out.println("s: " + s);
        System.out.println("isValid Status: " + isValid(board, i, j, board[i][j]));

        if (board[i][j] != '.') {
            System.out.println("Cell already filled. Moving to the next.");
            return dfs(board, s + 1);
        }

        for (char c = '1'; c <= '9'; ++c) {
            if (isValid(board, i, j, c)) {
                System.out.println("Trying to place '" + c + "' at (" + i + ", " + j + ")");
                board[i][j] = c;
                if (dfs(board, s + 1)) {
                    System.out.println("Successfully placed '" + c + "' at (" + i + ", " + j + ")");
                    return true;
                }
                System.out.println("Backtracking from (" + i + ", " + j + ")");
                board[i][j] = '.';
            }
        }

        System.out.println("No valid number to place. Backtracking from (" + i + ", " + j + ")");
        return false;
    }

    private static  boolean isValid(char[][] board, int row, int col, char c) {
        for (int i = 0; i < 9; ++i)
            if (board[i][col] == c || board[row][i] == c ||
                board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c)
                return false;
        return true;
    }

    private static  void printBoard(char[][] board) {
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        

        // Custom input
        char[][] customInput = {
            {'2', '5', '.', '.', '.', '3', '.', '.', '.'},
            {'.', '.', '.', '.', '.', '.', '2', '7', '.'},
            {'8', '7', '.', '.', '.', '6', '4', '.', '.'},
            {'.', '2', '.', '.', '.', '8', '1', '9', '3'},
            {'.', '1', '5', '.', '4', '.', '8', '.', '.'},
            {'.', '.', '.', '1', '.', '.', '.', '.', '4'},
            {'.', '.', '.', '7', '3', '4', '.', '.', '.'},
            {'.', '.', '.', '6', '.', '.', '.', '.', '9'},
            {'.', '6', '4', '.', '.', '9', '.', '5', '8'}
        };

        // Solve Sudoku
        sudokuSolver(customInput);
    }
}


```
