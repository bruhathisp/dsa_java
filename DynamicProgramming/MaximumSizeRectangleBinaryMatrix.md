## Solution

[Watch this](https://www.codingninjas.com/studio/library/maximum-size-of-rectangle-in-a-binary-matrix)

[Video explanation for histogram area](https://youtu.be/X0X6G-eWgQ8?feature=shared)

![image](https://github.com/bruhathisp/dsa_java/assets/91585301/e00bac24-fbf3-4de1-801f-562df1920a8a)


Lessons Learnt:
1. When we're calculating the area of a rectangle in a histogram, choosing the nearest smaller bars ensures that the width of the rectangle includes only bars that are equal or shorter in height. This helps us accurately represent the space enclosed by bars of similar or smaller heights, and it prevents the rectangle from extending too far to include taller bars, which would reduce the overall area. It's a key concept in finding the maximum rectangular area within a histogram.

2. `stack.peek()` the last element in the stack. because it is LIFO. `stack.pop` gives the most recent element added to stack LIFO.

   ### Explanation


The expression `histogram[i][j] = A[i][j] == 1 ? histogram[i - 1][j] + 1 : 0;` is a ternary operator used to populate the `histogram` array. Let's break down the logic behind this expression:

- `A[i][j] == 1`: This part checks if the element in the original matrix `A` at position `(i, j)` is equal to 1. It essentially checks if there is a '1' in the original matrix at the current position.

- `histogram[i - 1][j] + 1`: If the condition `A[i][j] == 1` is true, it means there is a '1' in the current position. In this case, the value for the corresponding position in the `histogram` array is set to one more than the value at the same column in the row above (`histogram[i - 1][j]`). This is because we are counting consecutive '1's, and adding 1 to the height of the bar.

- For each row, the maximum rectangle area is calculated using the `largestRectangleArea` function. The overall maximum rectangle area is updated if a larger area is found. For histogram `[[1, 1, 1], [0, 2, 2], [1, 0, 0]]`
- Row 1: Largest rectangle area is 3 (from (0,1), (0,2), (1,1))
- Row 2: Largest rectangle area is 2 (from (1,1), (1,2))
- Row 3: Largest rectangle area is 1 (from (2,0))
- The overall maximum rectangle area is 3, so the answer is 3.



## `largestRectangleArea` explained
#### Left array (find the index of the nearest smaller element on the left for each element)
- Iterate for each element in the row and maintain a stack where you push the value of i.   0->1->2
- Pop elements from the stack until it finds a smaller element or until the stack becomes empty.
- If the stack is empty insert -1 to the `left[i]`. Else insert the `stack.peek()`

Clear the Stack before right array. 

#### Right  array (find the index of the nearest smaller element on the right for each element)
- Iterate for each element in the row `reverse order`  and maintain a stack where you push the value of i. 2->1->0->
- Pop elements from the stack until it finds a smaller element or until the stack becomes empty.
- If the stack is empty insert n to the `right[i]`. Else insert the `stack.peek()`

#### Maximum area 
-Itherate till n.
- `max(maxArea, heights[i] * (right[i] - left[i] - 1));`
- return `maxArea`

### Code 

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
### Output

int[][] A = new int[][]{{1, 1, 1}, {0, 1, 1}, {1, 0, 0}};

[[1, 1, 1], [0, 2, 2], [1, 0, 0]]

Row 0: [1, 1, 1]
Left Stack Before Loop: []

Iteration 0
Left Stack During Loop: [0]
Left Array: [-1, 0, 0]

Iteration 1
Popped from left stack: 0
Stack empty so add -1 to left
Left Stack During Loop: [1]
Left Array: [-1, -1, 0]

Iteration 2
Popped from left stack: 1
Stack empty so add -1 to left
Left Stack During Loop: [2]
Left Array: [-1, -1, -1]

 Updating Right array
The stack: [] Value of n: 3
Stack empty so add n to right n: 3
Pushing value of i into stack: 2 The stack: [2]
Right Array: [0, 0, 3]
   The stack: [2]
Popped from right stack: 2   The stack now: []
Stack empty so add n to right n: 3
Pushing value of i into stack: 1 The stack: [1]
Right Array: [0, 3, 3]
   The stack: [1]
Popped from right stack: 1   The stack now: []
Stack empty so add n to right n: 3
Pushing value of i into stack: 0 The stack: [0]
Right Array: [3, 3, 3]

 Updating Max Area
Heights: [1, 1, 1]   Right: [3, 3, 3]   Left: [-1, -1, -1]
Max Area for this iteration: 0 is 3
Max Area for this iteration: 1 is 3
Max Area for this iteration: 2 is 3
Max Area for this row: 3

Row 1: [0, 2, 2]
Left Stack Before Loop: []

Iteration 0
Left Stack During Loop: [0]
Left Array: [-1, 0, 0]

Iteration 1
Left Stack During Loop: [0, 1]
Left Array: [-1, 0, 0]

Iteration 2
Popped from left stack: 1
Left Stack During Loop: [0, 2]
Left Array: [-1, 0, 0]

 Updating Right array
The stack: [] Value of n: 3
Stack empty so add n to right n: 3
Pushing value of i into stack: 2 The stack: [2]
Right Array: [0, 0, 3]
   The stack: [2]
Popped from right stack: 2   The stack now: []
Stack empty so add n to right n: 3
Pushing value of i into stack: 1 The stack: [1]
Right Array: [0, 3, 3]
   The stack: [1]
Popped from right stack: 1   The stack now: []
Stack empty so add n to right n: 3
Pushing value of i into stack: 0 The stack: [0]
Right Array: [3, 3, 3]

 Updating Max Area
Heights: [0, 2, 2]   Right: [3, 3, 3]   Left: [-1, 0, 0]
Max Area for this iteration: 0 is 0
Max Area for this iteration: 1 is 4
Max Area for this iteration: 2 is 4
Max Area for this row: 4

Row 2: [1, 0, 0]
Left Stack Before Loop: []

Iteration 0
Left Stack During Loop: [0]
Left Array: [-1, 0, 0]

Iteration 1
Popped from left stack: 0
Stack empty so add -1 to left
Left Stack During Loop: [1]
Left Array: [-1, -1, 0]

Iteration 2
Popped from left stack: 1
Stack empty so add -1 to left
Left Stack During Loop: [2]
Left Array: [-1, -1, -1]

 Updating Right array
The stack: [] Value of n: 3
Stack empty so add n to right n: 3
Pushing value of i into stack: 2 The stack: [2]
Right Array: [0, 0, 3]
   The stack: [2]
Popped from right stack: 2   The stack now: []
Stack empty so add n to right n: 3
Pushing value of i into stack: 1 The stack: [1]
Right Array: [0, 3, 3]
Stack is not empty so add stack.peek to right: 1
Pushing value of i into stack: 0 The stack: [1, 0]
Right Array: [1, 3, 3]

 Updating Max Area
Heights: [1, 0, 0]   Right: [1, 3, 3]   Left: [-1, -1, -1]
Max Area for this iteration: 0 is 1
Max Area for this iteration: 1 is 1
Max Area for this iteration: 2 is 1
Max Area for this row: 1
4



## Code to create the above 


``` java
            import java.util.*;

public class MyClass {
    public static int maximalRectangle(int[][] A) {
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
                    histogram[i][j] = A[i][j];
                } else {
                    histogram[i][j] = A[i][j] == 1 ? histogram[i - 1][j] + 1 : 0;
                }
            }
        }

        int maxArea = 0;

        System.out.println(Arrays.deepToString(histogram));

        // Iterate through each row and calculate the max area using the histogram
        for (int i = 0; i < n; i++) {
            System.out.println("\nRow " + i + ": " + Arrays.toString(histogram[i]));
            maxArea = Math.max(maxArea, largestRectangleArea(histogram[i]));
        }

        return maxArea;
    }

    // Function to find the largest rectangle area in a histogram
    private static int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Stack<Integer> stack = new Stack<>();

        // Display the contents of the left stack before the loop
        System.out.println("Left Stack Before Loop: " + stack);

        // Calculate the nearest smaller element on the left for each element
        for (int i = 0; i < n; i++) {
            System.out.println("\nIteration " + i);
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                System.out.println("Popped from left stack: " + stack.pop());
                if (stack.isEmpty()) {
                    System.out.println("Stack empty so add -1 to left");
                }
            }
            left[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
            System.out.println("Left Stack During Loop: " + stack);
            System.out.println("Left Array: " + Arrays.toString(left));
        }
        
        
        
        System.out.println("\n Updating Right array");

        // Clear the stack for the next iteration
        stack.clear();

        System.out.println("The stack: " + stack + " Value of n: " + n);

        // Calculate the nearest smaller element on the right for each element
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                System.out.println("   The stack: " + stack);
                System.out.println("Popped from right stack: " + stack.pop() + "   The stack now: " + stack);
            }

            if (stack.isEmpty()) {
                System.out.println("Stack empty so add n to right n: " + n);
            } else {
                System.out.println("Stack is not empty so add stack.peek to right: " + stack.peek());
            }
            right[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
            System.out.println("Pushing value of i into stack: " + i + " The stack: " + stack);
            System.out.println("Right Array: " + Arrays.toString(right));
        }
        System.out.println("\n Updating Max Area");
       
        // Calculate the maximum area
        int maxArea = 0;

        System.out.println("Heights: " + Arrays.toString(heights) + "   Right: " + Arrays.toString(right) + "   Left: " + Arrays.toString(left));

        for (int i = 0; i < n; i++) {
            maxArea = Math.max(maxArea, heights[i] * (right[i] - left[i] - 1));
            System.out.println("Max Area for this iteration: " + i + " is " + maxArea);
        }

        System.out.println("Max Area for this row: " + maxArea);
        return maxArea;
    }

    public static void main(String args[]) {
        int[][] A = new int[][]{{1, 1, 1}, {0, 1, 1}, {1, 0, 0}};
        System.out.println(maximalRectangle(A));
    }
}

```

