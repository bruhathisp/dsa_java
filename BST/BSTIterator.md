## Solution

Steps: 

1. Do the inorder traversal.
2. Initialise a new stack and create a function to push left nodes to the stack. `while (root != null) --> stack.push(root);-->root = root.left;`
3. In the `hasNext` function create a simple return that checks whether the stack is not empty `!stack.isEmpty()`
4. In the `next()` function, pop the top node from the stack i.e. `node` and push the nodes on the right tree, (Left, root, right) `pushLeftNodes(node.right);` then return the node.

```
    2
   / \
  1   3
       \
        4
         \
          5
           \
            7

Output: 1 2 3 4 5 7



      6
    /   \
   2     9
  / \      \
 1    5     8
     /       \
    4         7

Output 1 2 4 5 6 7 8 9


     7
    / \
   3  12
  / \   \
 1   5  11
    /     \
   4       10

Output 1 3 4 5 7 10 11 12

    28
   / 
  14   
     \
     11

Output 11 14 28



```


``` java
class BSTIterator {
    private Stack<Node> stack;

    // Constructor
    public BSTIterator(Node root) {
        stack = new Stack<>();
        // Call the helper method to push nodes to the stack
        pushLeftNodes(root);
    }

    // Check if there is a next element in the traversal
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    // Move the pointer to the next node in the traversal and return the node
    public Node next() {
        // Pop the top node from the stack
        Node node = stack.pop();
        // Call the helper method to push nodes of the right subtree
        pushLeftNodes(node.right);
        return node;
    }

    // Helper method to push nodes of the left subtree onto the stack
    private void pushLeftNodes(Node root) {
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }
}


```


## Example

```

     7
    / \
   3  12
  / \   \
 1   5  11
    /     \
   4       10

Output 1 3 4 5 7 10 11 12

Pushing to stack: 7

Pushing to stack: 3

Pushing to stack: 1
Updated stack: [7, 3, 1]
Left Node: null
Right Node: null

Complete Inorder Traversal:
Updated stack: [7, 3]
Left Node: null
Right Node: null
1 

Pushing to stack: 5

Pushing to stack: 4
Updated stack: [7, 5, 4]
Left Node: null
Right Node: null
3 
Updated stack: [7, 5]
Left Node: null
Right Node: null
4 
Updated stack: [7]
Left Node: null
Right Node: null
5 

Pushing to stack: 12

Pushing to stack: 11

Pushing to stack: 10
Updated stack: [12, 11, 10]
Left Node: null
Right Node: null
7 
Updated stack: [12, 11]
Left Node: null
Right Node: null
10 
Updated stack: [12]
Left Node: null
Right Node: null
11 
Updated stack: []
Left Node: null
Right Node: null
12 



```




``` java
import java.util.*;

public class MyClass {

    public static class Node {
        public Node left;
        public Node right;
        public int data;

        public Node(int data) {
            this.data = data;
        }
    }

    private static Stack<Node> stack;

    public MyClass(Node root) {
        stack = new Stack<>();
        pushLeftNodes(root);
    }

    public static boolean hasNext() {
        return !stack.isEmpty();
    }

    public static Node next() {
        Node node = stack.pop();
        pushLeftNodes(node.right);
        return node;
    }

    private static void pushLeftNodes(Node root) {
        while (root != null) {
            System.out.println("Pushing to stack: " + root.data);
            stack.push(root);
            root = root.left;
        }
        System.out.println("Updated stack: " + stackToString());
        System.out.println("Left Node: " + (root != null ? root.data : "null"));
        System.out.println("Right Node: " + (root != null && root.right != null ? root.right.data : "null"));
    }

    private static String stackToString() {
        StringBuilder result = new StringBuilder("[");
        for (Node node : stack) {
            result.append(node.data).append(", ");
        }
        if (!stack.isEmpty()) {
            result.setLength(result.length() - 2); // Remove the trailing comma and space
        }
        result.append("]");
        return result.toString();
    }

    public static void main(String args[]) {

        Node root = new Node(7);
        root.left = new Node(3);
        root.right = new Node(12);
        root.left.left = new Node(1);
        root.left.right = new Node(5);
        root.right.left = new Node(11);
        root.right.right = null; // Assuming the remaining nodes are null

        MyClass iterator = new MyClass(root);

        while (iterator.hasNext()) {
            Node nextNode = iterator.next();
            System.out.print(nextNode.data + " ");
        }
    }
}


```
