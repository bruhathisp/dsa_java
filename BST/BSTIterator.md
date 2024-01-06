## Solution


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

// import java.util.Stack;

// class Node {
//     public Node left;
//     public Node right;
//     public int data;

//     public Node(int data) {
//         this.data = data;
//     }
// }

class BSTIterator {
    private Stack<Node> stack;

    public BSTIterator(Node root) {
        stack = new Stack<>();
        // Initialize the stack with nodes along the leftmost path
        populateStack(root);
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    public Node next() {
        if (!hasNext()) {
            throw new IllegalStateException("No next element");
        }

        // Node at the top of the stack is the next node in the inorder traversal
        Node current = stack.pop();
        // Add nodes from the right subtree of the current node to the stack
        populateStack(current.right);

        return current;
    }

    private void populateStack(Node node) {
        // Add nodes along the leftmost path to the stack
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}

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
