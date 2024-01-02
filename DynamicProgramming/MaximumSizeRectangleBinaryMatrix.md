## Solution

[Watch this](https://www.codingninjas.com/studio/library/maximum-size-of-rectangle-in-a-binary-matrix)

The expression `histogram[i][j] = A[i][j] == 1 ? histogram[i - 1][j] + 1 : 0;` is a ternary operator used to populate the `histogram` array. Let's break down the logic behind this expression:

- `A[i][j] == 1`: This part checks if the element in the original matrix `A` at position `(i, j)` is equal to 1. It essentially checks if there is a '1' in the original matrix at the current position.

- `histogram[i - 1][j] + 1`: If the condition `A[i][j] == 1` is true, it means there is a '1' in the current position. In this case, the value for the corresponding position in the `histogram` array is set to one more than the value at the same column in the row above (`histogram[i - 1][j]`). This is because we are counting consecutive '1's, and adding 1 to the height of the bar.


``` java
      public class Solution {
    public int maximalRectangle(int[][] A) {
        if (A == null || A.length == 0 || A[0].length == 0) {
            return 0;
        }

        int n = A.length;
        int m = A[0].length;

        // Convert the matrix to a histogram
        int[][] histogram = new int[n][m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (i == 0) {
                    // for first row i=0 the value is fed into histogram
                    histogram[i][j] = A[i][j];
                } else {
                    histogram[i][j] = A[i][j] == 1 ? histogram[i - 1][j] + 1 : 0;
                }
            }
        }

        int maxArea = 0;

        // Iterate through each row and calculate the max area using the histogram
        for (int i = 0; i < n; i++) {
            maxArea = Math.max(maxArea, largestRectangleArea(histogram[i]));
        }

        return maxArea;
    }

    // Function to find the largest rectangle area in a histogram
    private int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Stack<Integer> stack = new Stack<>();

        // Calculate the nearest smaller element on the left for each element
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                stack.pop();
            }
            left[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }

        // Clear the stack for the next iteration
        stack.clear();

        // Calculate the nearest smaller element on the right for each element
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                stack.pop();
            }
            right[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
        }

        // Calculate the maximum area
        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            maxArea = Math.max(maxArea, heights[i] * (right[i] - left[i] - 1));
        }

        return maxArea;
    }
}

```
