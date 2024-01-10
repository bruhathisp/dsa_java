## Solution



Lessons Learnt:
1. The recursive nature of this solution explores all possible combinations by trying different configurations of queens on the chessboard. If a solution is found, it is added to the result list. If not, the algorithm backtracks to explore other possibilities.
2. In Java, arrays are mutable, which means you can modify their elements after the array is created. The line `char[][] board = new char[n][n];` but strings are immutable. This means that once a String object is created, its state (the sequence of characters it represents) cannot be changed. 
3. use



Steps:
1. The function takes `List<List<String>>` as return value. so initialise the result `List<List<String>> result = new ArrayList<>();`
2. `char[][] board = new char[n][n];` is a nxn matrix  of characters with initial value of `.`
3. Just call the helper function  `solveNQueensHelper`with the initial board, first row and result matrix `solveNQueensHelper(board, 0, result);` and return the `result`
### void solveNQueensHelper `    private void solveNQueensHelper(char[][] board, int row, List<List<String>> result) {
`
1. Check `if (row == board.length)` add the current board to the result and terminate. (It is a void function and when it terminates, we have the final solution)
2. Iterate through the columns, check `if (isValid(board, row, col))` and place the queen in the board, now there are 2 possibilities, explore further or backtrack. Explore-->`solveNQueensHelper(board, row + 1, result);` Backtrack --> board[row][col] = '.'; (that means it goes to the next column at the end of its iteration)

### boolean isValid(char[][] board, int row, int col) 
1. Queen in the same column
   1. Iterate through the rows. `for (int i = 0; i < row; i++)`
   2. Check `if (board[i][col] == 'Q') ` True --> return false (not a valid board)
2. Queen in the same diagonal (upper left to lower right
   1. Iterate through the diagonal from lower right to upper left. `for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)`
      1. i = row - 1, j = col - 1
      2. i >= 0 && j >= 0
      3. i--, j--
   3. Check `if (board[i][j] == 'Q') ` True --> return false (not a valid board)
3. Queen in the same diagonal (upper right to lower left
   1. Iterate through the diagonal from lower left to upper right. `for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++)`
      1. i = row - 1, j = col + 1;
      2. i >= 0 && j < board.length;
      3. i--, j++
   2. Check `if (board[i][j] == 'Q') ` True --> return false (not a valid board)
4. return true

So when all the rows are done being iterated, the `row` variable will be equal to board.length and it means that we got a valid solution. now add it to the result. But board is `char[][]` type. result is `List<List<String>>`. Use the `constructSolution` method.

### List<String> constructSolution(char[][] board)
1. **What does it do?** returns a valid solution in a list of Strings. that is each row is a string and one single valid solution is a list of strings.
2.  ``` java
	private List<String> constructSolution(char[][] board) {
        List<String> solution = new ArrayList<>();
        for (char[] row : board) {
            solution.add(new String(row));
        }
        return solution;
   	 }
    ```



## Code 

``` java
class Solution {
	List<List<String>> getNQueensSolutions(int n) {
		
		List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }

        solveNQueensHelper(board, 0, result);
        return result;
    }

    private void solveNQueensHelper(char[][] board, int row, List<List<String>> result) {
        if (row == board.length) {
            result.add(constructSolution(board));
            return;
        }

        for (int col = 0; col < board.length; col++) {
            if (isValid(board, row, col)) {
                board[row][col] = 'Q';
                solveNQueensHelper(board, row + 1, result);
                board[row][col] = '.';
            }
        }
    }

    private boolean isValid(char[][] board, int row, int col) {
        // Check if there is a queen in the same column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check if there is a queen in the same diagonal (upper left to lower right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Check if there is a queen in the same diagonal (upper right to lower left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        return true;
    }

    private List<String> constructSolution(char[][] board) {
        List<String> solution = new ArrayList<>();
        for (char[] row : board) {
            solution.add(new String(row));
        }
        return solution;
    }
	    // add your logic here
	}


```

```
Placing queen at row 0, col 0
Q...
....
....
....

Exploring further...
Invalid move: Queen in the same column at row 0, col 0
Invalid move: Queen in the same diagonal (upper left to lower right) at row 0, col 0
Placing queen at row 1, col 2
Q...
..Q.
....
....

Exploring further...
Invalid move: Queen in the same column at row 0, col 0
Invalid move: Queen in the same diagonal (upper right to lower left) at row 1, col 2
Invalid move: Queen in the same column at row 1, col 2
Invalid move: Queen in the same diagonal (upper left to lower right) at row 1, col 2
Backtracking...
Q...
....
....
....

Placing queen at row 1, col 3
Q...
...Q
....
....

Exploring further...
Invalid move: Queen in the same column at row 0, col 0
Placing queen at row 2, col 1
Q...
...Q
.Q..
....

Exploring further...
Invalid move: Queen in the same column at row 0, col 0
Invalid move: Queen in the same column at row 2, col 1
Invalid move: Queen in the same diagonal (upper left to lower right) at row 2, col 1
Invalid move: Queen in the same column at row 1, col 3
Backtracking...
Q...
...Q
....
....

Invalid move: Queen in the same diagonal (upper left to lower right) at row 0, col 0
Invalid move: Queen in the same column at row 1, col 3
Backtracking...
Q...
....
....
....

Backtracking...
....
....
....
....

Placing queen at row 0, col 1
.Q..
....
....
....

Exploring further...
Invalid move: Queen in the same diagonal (upper right to lower left) at row 0, col 1
Invalid move: Queen in the same column at row 0, col 1
Invalid move: Queen in the same diagonal (upper left to lower right) at row 0, col 1
Placing queen at row 1, col 3
.Q..
...Q
....
....

Exploring further...
Placing queen at row 2, col 0
.Q..
...Q
Q...
....

Exploring further...
Invalid move: Queen in the same column at row 2, col 0
Invalid move: Queen in the same column at row 0, col 1
Placing queen at row 3, col 2
.Q..
...Q
Q...
..Q.

Exploring further...
Constructed Row: .Q..
Constructed Row: ...Q
Constructed Row: Q...
Constructed Row: ..Q.
Found Solution:
.Q..
...Q
Q...
..Q.

(See the entire output with the code)

Invalid move: Queen in the same column at row 0, col 3
Backtracking...
...Q
....
....
....

Placing queen at row 1, col 1
...Q
.Q..
....
....

Exploring further...
Invalid move: Queen in the same diagonal (upper right to lower left) at row 1, col 1
Invalid move: Queen in the same column at row 1, col 1
Invalid move: Queen in the same diagonal (upper left to lower right) at row 1, col 1
Invalid move: Queen in the same column at row 0, col 3
Backtracking...
...Q
....
....
....

Invalid move: Queen in the same diagonal (upper right to lower left) at row 0, col 3
Invalid move: Queen in the same column at row 0, col 3
Backtracking...
....
....
....
....



// constructSolution functioning 

[[.Q.., ...Q, Q..., ..Q.], [..Q., Q..., ...Q, .Q..]]


```


## Code to print 


``` java
	import java.util.*;

public class MyClass {

    public static void main(String[] args) {
        // Call the method on an instance of the class
        MyClass myClass = new MyClass();
        System.out.println(myClass.getNQueensSolutions(4));
    }

    List<List<String>> getNQueensSolutions(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        // Initialize the chessboard
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }

        // Start finding solutions
        solveNQueensHelper(board, 0, result);
        return result;
    }

    private void solveNQueensHelper(char[][] board, int row, List<List<String>> result) {
        // If we reached the end of the board, add the solution
        if (row == board.length) {
            result.add(constructSolution(board));
            // Print the current solution
            System.out.println("Found Solution:");
            printBoard(board);
            System.out.println();
            return;
        }

        // Try placing a queen in each column of the current row
        for (int col = 0; col < board.length; col++) {
            if (isValid(board, row, col)) {
                board[row][col] = 'Q';
                // Print the current state of the board after placing a queen
                System.out.println("Placing queen at row " + row + ", col " + col);
                printBoard(board);

                // Continue exploring with the queen placed
                System.out.println("Exploring further...");
                solveNQueensHelper(board, row + 1, result);

                // Backtrack: remove the queen to explore other possibilities
                System.out.println("Backtracking...");
                board[row][col] = '.';
                printBoard(board);
            }
        }
    }

    private boolean isValid(char[][] board, int row, int col) {
        // Check if there is a queen in the same column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') {
                System.out.println("Invalid move: Queen in the same column at row " + i + ", col " + col);
                return false;
            }
        }

        // Check if there is a queen in the same diagonal (upper left to lower right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                System.out.println("Invalid move: Queen in the same diagonal (upper left to lower right) at row " + i + ", col " + j);
                return false;
            }
        }

        // Check if there is a queen in the same diagonal (upper right to lower left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
            if (board[i][j] == 'Q') {
                System.out.println("Invalid move: Queen in the same diagonal (upper right to lower left) at row " + i + ", col " + j);
                return false;
            }
        }

        return true;
    }

private List<String> constructSolution(char[][] board) {
    List<String> solution = new ArrayList<>();
    for (char[] row : board) {
        // Convert the char array of a row to a string and add it to the solution list
        String rowString = new String(row);
        solution.add(rowString);

        // Print the current row
        System.out.println("Constructed Row: " + rowString);
    }
    return solution;
}


    // Utility method to print the current state of the chessboard
    private void printBoard(char[][] board) {
        for (char[] row : board) {
            System.out.println(new String(row));
        }
        System.out.println();
    }
}


```
