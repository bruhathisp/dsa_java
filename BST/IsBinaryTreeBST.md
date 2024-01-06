## Solution

Lesson Learnt:

The reason `prev` is an array in this implementation is to provide a way to simulate passing by reference in Java. In Java, when you pass a primitive data type (like `int`) to a method, it is passed by value, not by reference. This means that changes made to the parameter inside the method do not affect the original value outside the method.

By using an array (even an array with a single element), you can modify the content of the array inside the method, and those changes will be reflected outside the method. This allows you to effectively pass a reference to an integer-like value.

Here's a breakdown of how it works:

1. `int[] prev` is declared as an array of integers (length 1).

2. The method `isBst` initializes this array with `int[] prev = { Integer.MIN_VALUE };`.

3. The array is passed to the method `isBstUtil`.

4. Inside `isBstUtil`, changes to `prev[0]` are tracked, and this value is used as the "previous" value during the tree traversal.

Using an array (even with just one element) allows the method to modify the content of the array (in this case, the value at index 0), and this modification is reflected outside the method. If a simple `int` was used, modifications to it inside the method would not affect the original value outside the method.

If you prefer to use a single integer, you could modify the code accordingly, but it would involve returning the updated `prev` value from each recursive call, and it might make the code less readable. The array approach is a common workaround in Java for simulating pass-by-reference behavior.

Steps:
1. If node is null return true. (Null tree can be a BST)
2. Check if `isBstUtil(node.left,prev)` is true. If it is not true, it is not a BST.
3. If the node is less then the first element in prev, it is not a BST (There is only one element, you need an array because pass by reference)
4. Update the prev[0] `prev[0] = node.data;`
5. Return `isBstUtil(node.right, prev);`



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

    boolean isBst(Node root) {
        // Use an integer to store the previous value during traversal
        int[] prev = {Integer.MIN_VALUE};
        return isBstUtil(root, prev);
    }

    boolean isBstUtil(Node node, int prev[]) {
        // Base case: an empty tree is a BST
        if (node == null) {
            return true;
        }

        // Check the left subtree
        if (!isBstUtil(node.left, prev)) {
            return false;
        }

        // Check the current node's value
        if (node.data <= prev[0]) {
            return false;
        }

        // Update the previous value
        prev[0] = node.data;

        // Check the right subtree
        return isBstUtil(node.right, prev);
    }
}

```
