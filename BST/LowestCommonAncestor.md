## Solution

Lessons Learnt:
1. To check whether a root is null use `root==null` and not  `root.data==null`
2. It is best practise to not use `p.data` while checking the information about the position of p and q in the tree. Base case.



The problem is to find the Lowest Common Ancestor (LCA) of two nodes, `p` and `q`, in a Binary Search Tree (BST). The LCA is the node from which both `p` and `q` are descendants.

Here's a simple explanation of the Java code:

1. **Base Case:**
   - If the root is `null` or one of the nodes (`p` or `q`) is the root itself, return the root. This is because the LCA in these cases would be the root.

2. **Search in Left Subtree:**
   - If both `p` and `q` are smaller than the current root, it means they are in the left subtree. So, call the `lowestCommonAncestor` function recursively on the left subtree.

3. **Search in Right Subtree:**
   - If both `p` and `q` are greater than the current root, it means they are in the right subtree. So, call the `lowestCommonAncestor` function recursively on the right subtree.

4. **LCA Found:**
   - If one node is on the left and the other is on the right, the current root is the Lowest Common Ancestor. Return the current root.

This recursive approach navigates through the BST, narrowing down the search based on the values of `p` and `q`, until it finds the lowest common ancestor.

Example:
```
     5
    / \
   2   8
  / \ / \
 1  3 7  9

For nodes 1 and 3:
LCA(1, 3) = 2

For nodes 7 and 9:
LCA(7, 9) = 8
```



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

    Node lowestCommonAncestor(Node root, Node p, Node q) {
        // Base case: If the root is null or one of the nodes is the root, return the root
        if (root == null || root == p || root == q) {
            return root;
        }

        // If both nodes are less than the root, search in the left subtree
        if (p.data < root.data && q.data < root.data) {
            return lowestCommonAncestor(root.left, p, q);
        }

        // If both nodes are greater than the root, search in the right subtree
        if (p.data > root.data && q.data > root.data) {
            return lowestCommonAncestor(root.right, p, q);
        }

        // If one node is on the left and the other is on the right, root is the LCA
        return root;
    }
}
