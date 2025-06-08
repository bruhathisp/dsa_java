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


> "If we already found an increasing path from `(i, j)` to `(x, y)`, why do we still do `maxPath = Math.max(maxPath, 1 + dfs(...))`? Shouldn’t we just return `1 + dfs(...)`?"

Let’s clear this up **simply** and **step by step**.

---

## 🔍 You're Right That...

Yes — when we call:

```java
1 + dfs(matrix, x, y)
```

We're getting **a valid increasing path** by going from `(i, j)` ➝ `(x, y)` ➝ ...longest path from there.

So you're wondering:

> "Why do we need to check `Math.max()`? Isn’t there only one path?"

---

## 💡 The Key Realization

From cell `(i, j)`, we might have **many valid directions** to move:

```java
for (each direction) {
    if (matrix[x][y] > matrix[i][j]) {
        maxPath = Math.max(maxPath, 1 + dfs(matrix, x, y));
    }
}
```

You're choosing the **best** one.

> We take the `max` because **more than one increasing path can start from a cell**, and **we want the longest one**.

---

## 🧪 Example: Multiple Choices

Matrix:

```
[ [1, 2, 3],
  [6, 5, 4] ]
```

Start at `(1, 0)` → value = 6

Valid directions:

* Up → `(0,0)` = 1 ❌ (not increasing)
* Right → `(1,1)` = 5 ❌ (not increasing)

Wait... what about `(1, 0)`? That’s a **dead end** — longest path is just `6` ➝ length 1.

Now consider `(0, 0)` = 1

* Right → 2 → 3 → 4 → 5 → 6

That’s a long path!

So from `(0,0)`:

* One option: go right to `(0,1)` → keep growing
* Another option: go down to `(1,0)` → valid, but shorter path

Thus:

```java
maxPath = Math.max(maxPath, 1 + dfs(next))
```

picks **the best** among all possible valid next steps.

---

## ✅ Final Intuition

Even if one direction gives a valid increasing path,

> **another direction might give an even longer path**.

So, we:

* Try all 4 directions (only if they go to a bigger number)
* Use `Math.max()` to find the **longest path among them**

---

## 📌 Final Answer to Your Question

> **We take `Math.max(...)` because there may be multiple increasing paths starting from `(i, j)` — and we want the longest one**.



