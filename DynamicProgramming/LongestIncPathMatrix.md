# Solution 

``` java

public class Solution {
    // Directions: down, up, right, left
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int rows, cols;
    int[][] memo;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;

        rows = matrix.length;
        cols = matrix[0].length;
        memo = new int[rows][cols];  // stores longest path from each cell

        int maxLen = 0;

        // Try starting DFS from every cell
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                maxLen = Math.max(maxLen, dfs(matrix, i, j));
            }
        }

        return maxLen;
    }

    private int dfs(int[][] matrix, int i, int j) {
        // If already computed, return the memoized value
        if (memo[i][j] != 0) return memo[i][j];

        int maxPath = 1; // At least the current cell

        // Try all 4 directions
        for (int[] dir : directions) {
            int x = i + dir[0];
            int y = j + dir[1];

            // Check bounds and increasing condition
            if (x >= 0 && x < rows && y >= 0 && y < cols &&
                matrix[x][y] > matrix[i][j]) {

                // Recurse and add 1 for the move
                maxPath = Math.max(maxPath, 1 + dfs(matrix, x, y));
            }
        }

        memo[i][j] = maxPath;  // Save the result
        return maxPath;
    }
}
```


Here, 

```java
maxPath = Math.max(maxPath, 1 + dfs(matrix, x, y));
```

This line is **the heart** of the DFS logic, and understanding it well will make the whole algorithm click. Let’s walk through it slowly.

---

## 🧠 What’s Our Goal?

We want to find:

> **The longest increasing path** starting from a given cell `(i, j)`.

To do that, we:

1. Try to move in all 4 directions (up/down/left/right),
2. But only to **cells with bigger values**.

---

## 💡 Let’s Break Down the Line

```java
1 + dfs(matrix, x, y)
```

Means:

> "If I go from `(i, j)` to `(x, y)`, what is the path length?"

* `dfs(matrix, x, y)` gives us the **longest path starting from (x, y)**.
* We add `1` because we **took one step** to get to `(x, y)`.

---

Now:

```java
maxPath = Math.max(maxPath, 1 + dfs(matrix, x, y));
```

Means:

> "Among all the paths I can take from `(i, j)`, what’s the longest one?"

* `maxPath` keeps track of the **best (longest)** path seen so far from `(i, j)`.
* We update it with the path length we just calculated.

---

## 🧪 Visual Example

Matrix:

```
[9, 9, 4]
[6, 6, 8]
[2, 1, 1]
```

Suppose we’re at cell `(2, 1)` which has value `1`.
We can move to `(2, 0)` which is `2` (increasing!).

Let’s say `dfs(2, 0)` returns `3` (because 2 → 6 → 9).

Then:

```java
1 + dfs(2, 0) = 1 + 3 = 4
```

We update:

```java
maxPath = Math.max(maxPath, 4);  // maxPath becomes 4
```

That’s the **longest increasing path starting at (2, 1)**.

---

## 🔁 This Happens for All Directions

You loop over all 4 directions:

* If the next cell is larger, calculate its path length.
* Add 1 (for your move), and take the max.

At the end, `maxPath` will hold the **longest increasing path** from the current cell.

---

## ✅ Final Summary

We use:

```java
maxPath = Math.max(maxPath, 1 + dfs(...));
```

Because:

* We are **trying all possible moves** from the current cell.
* We want to know **which path is longest**.
* We update our answer each time we find a longer one.

---

