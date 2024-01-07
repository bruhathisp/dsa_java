## Solution

Question
``` java
class Solution {
	/* This is the Node class definition
	
	class Node {
		public Node left;
		public Node right;
		public int data;

		public Node(int data) {
			this.data = data;
		}
	}
	*/

	int getLargestBstSize(Node root) {
	    // add your logic here
	}
}
```
Explanation:

Stpes:
**Lessons Learned:**

1. **Recursion for BST Validation:**
   - The `largestBst` method is a recursive function that traverses the tree and validates subtrees as BSTs.
   - Base case: An empty tree is considered a BST with size 0.
   - For each non-empty node, results for left and right subtrees are obtained recursively.
   - The current subtree is considered a BST if it satisfies the BST conditions:
     1. all nodes in the left subtree have values less than the root's value. `root.data > left.max`
     2. check if the left and right subtrees are valid BSTs.  `left.isBST && right.isBST `
     3. all nodes in the right subtree have values greater than the root's value. `root.data < right.min`
     - `left.isBST && right.isBST && root.data > left.max && root.data < right.min`
   - If the current subtree is a BST, the size is calculated, and min and max values are updated accordingly.                       ` return new Result(size, min, max, true);`

4. **Handling Non-BST Subtrees:**
   - If the current subtree is not a BST, information is returned for parent nodes. `return new Result(Math.max(left.size, right.size), 0, 0, false);`

6. **Returning Final Result:**
   - The `getLargestBstSize` method invokes the `largestBst` method on the root and returns the size of the largest BST.

**Code Explanation:**

- **`Result` Class:**
  - Defines a helper class `Result` to store information about a subtree, including size, min, max, and whether it is a valid BST.
  - The constructor initializes the values.

- **`getLargestBstSize` Method:**
  - Invokes the `largestBst` method on the root node.
  - Returns the size of the largest BST.

- **`largestBst` Method:**
  - **Base Case:** If the current node is null, returns a `Result` object representing an empty tree (size 0, valid BST).
  - Recursively obtains results for left and right subtrees.
  - Checks if the current subtree is a BST based on the conditions.
  - If valid, calculates size, updates min and max values, and returns a `Result` object.
  - If not valid, returns information for parent nodes (max size between left and right subtrees).

**Summary:**
The code uses a helper class (`Result`) to facilitate the recursive traversal of the tree, validating each subtree as a BST and calculating the size of the largest BST. The approach involves checking conditions for a valid BST and returning appropriate information for parent nodes when encountering non-BST subtrees. The `Math.min` and `Math.max` functions are utilized for updating min and max values. The final result is obtained by invoking the `largestBst` method on the root.

``` java
class Solution {
    class Result {
        int size;
        int min;
        int max;
        boolean isBST;

        public Result(int size, int min, int max, boolean isBST) {
            this.size = size;
            this.min = min;
            this.max = max;
            this.isBST = isBST;
        }
    }

    int getLargestBstSize(Node root) {
        Result result = largestBst(root);
        return result.size;
    }

    private Result largestBst(Node root) {
        // Base case: an empty tree is a BST with size 0
        if (root == null) {
            return new Result(0, Integer.MAX_VALUE, Integer.MIN_VALUE, true);
        }

        // Recursively get results for left and right subtrees
        Result left = largestBst(root.left);
        Result right = largestBst(root.right);

        // Check if the current subtree is a BST
        if (left.isBST && right.isBST && root.data > left.max && root.data < right.min) {
            int size = left.size + right.size + 1;
            int min = Math.min(root.data, left.min);
            int max = Math.max(root.data, right.max);
            return new Result(size, min, max, true);
        } else {
            // If the current subtree is not a BST, return information for parent nodes
            return new Result(Math.max(left.size, right.size), 0, 0, false);
        }
    }
}

```
