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
