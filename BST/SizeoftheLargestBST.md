## Solution


Explanation:

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
