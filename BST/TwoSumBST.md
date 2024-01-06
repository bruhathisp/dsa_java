## Solution



Steps:

1. Create 2 stacks  `stack1` for storing the left side elements of the node and `stack2` for storing the right side elements of the node.
2. Initialise 2 pointers `current1` indicating the minimum root and `current2` indicating the maximum root.  `Node current1 = root;` and `Node current2 = root;`
3. Traverse the node till `current1` becomes null and push current1.left into the stack1.
4. Traverse the node till `current2` becomes null and push current2.right into the stack2.
5. Traverse till
   1. stack 1 and 2 are not empty
   2. stack1.peek().data < stack2.peek().data
6. `sum = stack1.peek().data + stack2.peek().data;` if `sum=k` return sum
7. If `sum < k` --> pop the stack1 and `current1` is the right element of the root --> traverse till `current1` is null --> push the `current1` to stack1 --> update `current1` to the left element.
8. If `sum > k` --> pop the stack2 and `current2` is the left element of the root --> traverse till `current2` is null --> push the `current2` to stack2 --> update `current2` to the right element.
9. Atlast return false.



``` java
  class Solution {
    // Definition for a binary tree node.
//     class Node {
//         public Node left;
//         public Node right;
//         public int data;

//         public Node(int data) {
//             this.data = data;
//         }
//     }

    boolean pairExists(Node root, int k) {
        // In-order traversal using two stacks
        Stack<Node> stack1 = new Stack<>();
        Stack<Node> stack2 = new Stack<>();

        // Initialize pointers
        Node current1 = root;
        Node current2 = root;

        // Initial in-order push for stack1 (leftmost)
        while (current1 != null) {
            stack1.push(current1);
            current1 = current1.left;
        }

        // Initial in-order push for stack2 (rightmost)
        while (current2 != null) {
            stack2.push(current2);
            current2 = current2.right;
        }

        while (!stack1.isEmpty() && !stack2.isEmpty() && stack1.peek().data < stack2.peek().data) {
            int sum = stack1.peek().data + stack2.peek().data;

            if (sum == k) {
                return true;  // Pair found
            }

            if (sum < k) {
                // Move to the next node in the in-order traversal on the left
                current1 = stack1.pop().right;
                while (current1 != null) {
                    stack1.push(current1);
                    current1 = current1.left;
                }
            } else {
                // Move to the next node in the in-order traversal on the right
                current2 = stack2.pop().left;
                while (current2 != null) {
                    stack2.push(current2);
                    current2 = current2.right;
                }
            }
        }

        return false;  // No pair found
    }
}

```




Example:


        8
       / \
      3   9
       \   \
        4   10
             \
              12
                 \
                  11



```
Stack1:
Pushing to stack1: 8
Pushing to stack1: 3
stack1: [8, 3]

Stack2:
Pushing to stack2: 8
Pushing to stack2: 9
Pushing to stack2: 10
Pushing to stack2: 12
stack2: [8, 9, 10, 12]


sum 15      stack1.peek().data + stack2.peek().data3+12
sum > k
stack2: [8, 9, 10, 12]
current2: 11
Pushing to stack2: 11
stack2: [8, 9, 10, 11]
current2: null

sum 14      stack1.peek().data + stack2.peek().data3+11
sum > k
stack2: [8, 9, 10, 11]
current2: null

sum 13      stack1.peek().data + stack2.peek().data3+10
sum > k
stack2: [8, 9, 10]
current2: null

sum 12      stack1.peek().data + stack2.peek().data3+9
sum > k
stack2: [8, 9]
current2: null

sum 11      stack1.peek().data + stack2.peek().data3+8
sum > k
stack2: [8]
current2: 3
Pushing to stack2: 3
stack2: [3]
current2: 4
Pushing to stack2: 4
stack2: [3, 4]
current2: null

sum 7      stack1.peek().data + stack2.peek().data3+4

Pair exists: true
```



``` java
import java.util.Stack;

public class MyClass {
    // Definition for a binary tree node.
    public static class Node {
        public Node left;
        public Node right;
        public int data;

        public Node(int data) {
            this.data = data;
        }

        @Override
        public String toString() {
            return Integer.toString(data);
        }
    }

    static boolean pairExists(Node root, int k) {
        // In-order traversal using two stacks
        Stack<Node> stack1 = new Stack<>();
        Stack<Node> stack2 = new Stack<>();

        // Initialize pointers
        Node current1 = root;
        Node current2 = root;

        // Print the binary tree
        printBinaryTree(root);

        System.out.println("\nStack1:");
        // Initial in-order push for stack1 (leftmost)
        while (current1 != null) {
            System.out.println("Pushing to stack1: " + current1.data);
            stack1.push(current1);
            current1 = current1.left;
        }
        
        System.out.println("stack1: " + stack1);

        System.out.println("\nStack2:");
        // Initial in-order push for stack2 (rightmost)
        while (current2 != null) {
            System.out.println("Pushing to stack2: " + current2.data);
            stack2.push(current2);
            current2 = current2.right;
        }
        System.out.println("stack2: " + stack2);

        while (!stack1.isEmpty() && !stack2.isEmpty() && stack1.peek().data < stack2.peek().data) {
            int sum = stack1.peek().data + stack2.peek().data;
            System.out.println("sum "+ sum + "      stack1.peek().data + stack2.peek().data"+ (stack1.peek().data) + "+"+ (stack2.peek().data));
            if (sum == k) {
                return true; // Pair found
            }

            if (sum < k) {
                System.out.println("sum < k");
                System.out.println("stack1: " + stack1);
                // Move to the next node in the in-order traversal on the left
                // left root right
                current1 = stack1.pop().right;
                System.out.println("current1: " + current1);
                while (current1 != null) {
                    System.out.println("Pushing to stack1: " + current1.data);
                    stack1.push(current1);
                    System.out.println("stack1: " + stack1);
                    current1 = current1.left;
                    System.out.println("current1: " + current1);
                }
            } else {
                System.out.println("sum > k");
                System.out.println("stack2: " + stack2);
                // Move to the next node in the in-order traversal on the right
                current2 = stack2.pop().left;
                System.out.println("current2: " + current2);
                while (current2 != null) {
                    System.out.println("Pushing to stack2: " + current2.data);
                    stack2.push(current2);
                    System.out.println("stack2: " + stack2);
                    current2 = current2.right;
                    System.out.println("current2: " + current2);
                }
            }
        }

        return false; // No pair found
    }

    public static void main(String[] args) {
        // Example tree: 8 3 9 -1 4 -1 10 -1 -1 -1 12 11
        Node root = new Node(8);
        root.left = new Node(3);
        root.right = new Node(9);
        root.left.right = new Node(4);
        root.right.right = new Node(10);
        root.right.right.right = new Node(12);
        root.right.right.right.left = new Node(11);

        int k = 7;
        boolean result = pairExists(root, k);

        System.out.println("Pair exists: " + result);
    }

    private static void printBinaryTree(Node root) {
        System.out.println("Binary Tree Representation:");
        printBinaryTreeUtil(root, 0);
    }

    private static void printBinaryTreeUtil(Node root, int space) {
        if (root == null)
            return;

        space += 5;

        printBinaryTreeUtil(root.right, space);

        System.out.println();
        for (int i = 5; i < space; i++)
            System.out.print(" ");
        System.out.print(root.data + "\n");

        printBinaryTreeUtil(root.left, space);
    }
}

```
