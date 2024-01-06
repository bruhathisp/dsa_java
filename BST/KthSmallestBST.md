## Solution

Refer to the KthLargest

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

    int findKthSmallest(Node root, int k) {
        int[] count = {0};
        return findKthSmallestUtil(root, k, count);
    }

    int findKthSmallestUtil(Node node, int k, int[] count) {
        // Base case: an empty tree
        if (node == null) {
            return -1;
        }

        // Check the left subtree
        int leftResult = findKthSmallestUtil(node.left, k, count);
        if (leftResult != -1) {
            return leftResult;
        }

        // Increment count
        count[0]++;

        // Check if kth smallest is found
        if (count[0] == k) {
            return node.data;
        }

        // Check the right subtree
        return findKthSmallestUtil(node.right, k, count);
    }
}

```
