## Solution

Lessons Learnt:
1. Using `StringBuilder` in Java for string concatenation is more efficient than using the `+` operator or `String.concat()` method. 

In Java, strings are immutable, meaning that every time you perform a concatenation operation with the `+` operator or `String.concat()`, a new string object is created. This can be inefficient, especially when dealing with a large number of concatenations, as it leads to the creation of unnecessary intermediate string objects.

`StringBuilder` is mutable, meaning it can be modified without creating a new object every time you append or modify its content. This makes it more efficient for concatenating multiple strings, as it avoids unnecessary object creations.

In the context of serialization, where you are building a string representation of a tree node by node, using `StringBuilder` helps in reducing memory overhead and improving performance compared to using the `+` operator or `String.concat()` repeatedly.

2. In queue, you initialse with this statement (for integerqueue) `Queue<Integer> intQueue = new LinkedList<>();` and `order` to enqueue and `poll` to dequeue.
3. `intQueue.offer(10);` and `int firstElement = intQueue.poll();`
4. Split a string into an array of substrings based on the space character (' ').  `String[] values = serializedString.split(" ")`
5. Convert the first string in `values` to integer and add it to the Node `Node root = new Node(Integer.parseInt(values[0]));`

Steps: `String serialize(Node root) ` Level Order traversal

1. Initialise a `StringBuilder serializedString = new StringBuilder();`
2. If the root in null, just return `return serializedString.toString();`
3. Create a queue of type `Node` and enqueue the root of the tree.
4. Iterate till queue is not empty.
   1. `Node current = queue.poll();` is used to dequeue. The queue is used for `level-order traversal` of the binary search tree (BST). If the queue is empty, `current` will be `null`.
   2. `if (current != null)` -->append the data of the current node to the  `serializedString`. The space after `current.data` is added for better readability. `serializedString.append(current.data).append(" ");`
   3. `queue.offer(current.left);` and `queue.offer(current.right);` Enqueue the left and right child of the current node, if it exists, into the queue.
   4.  If `current` is `null`, meaning there is no valid node at this level, it appends `-1` to indicate a NULL node in the serialization.

Steps: `Node deserialize(String serializedString)`

1. Retrun null if `(serializedString == null || serializedString.isEmpty()) `
2. Split the serialized string into an array of substrings based on the space character (' ').  `String[] values = serializedString.split(" ")`
3. Return null if `values.length == 0`
4. Create the root node of the binary search tree (BST) based on the first value in the array of strings (values).`Node root = new Node(Integer.parseInt(values[0]));`
5. Enqueue the root to a queue of Node Type.
6. This loop starts from index 1 and iterates through the array with a step of 2, ensuring that it processes pairs of `values` at a time. The reason for this is that in a `level order traversal `of a BST, each parent node is followed by its left and right children.
   1. Dequeue a node from the queue because it is the `parent` of the current pair of `values`. `Node parent = queue.poll();`
   2. Check `if (!values[i].equals("-1"))`, it creates a new node and assign it as the left child of the parent node. The left child node is then enqueued for future processing.
      1. `parent.left = new Node(Integer.parseInt(values[i]))`
      2. `queue.offer(parent.left);`
   3. Check `if (i + 1 < values.length && !values[i + 1].equals("-1"))`
      1. `parent.right = new Node(Integer.parseInt(values[i + 1]))`
      2. `queue.offer(parent.right)`
7. Return `root`. Returning the entire tree (all nodes) is not necessary, as you can access any other node in the tree by traversing it starting from the root. 



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

    // Serialize BST to a string
    String serialize(Node root) {
        StringBuilder serializedString = new StringBuilder();
        if (root == null) {
            return serializedString.toString();
        }

        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            Node current = queue.poll();

            if (current != null) {
                serializedString.append(current.data).append(" ");

                queue.offer(current.left);
                queue.offer(current.right);
            } else {
                serializedString.append("-1 "); // Indicate a NULL node
            }
        }

        return serializedString.toString().trim();
    }

    // Deserialize a string to BST
    Node deserialize(String serializedString) {
        if (serializedString == null || serializedString.isEmpty()) {
            return null;
        }

        String[] values = serializedString.split(" ");
        if (values.length == 0) {
            return null;
        }

        Queue<Node> queue = new LinkedList<>();
        Node root = new Node(Integer.parseInt(values[0]));
        queue.offer(root);

        for (int i = 1; i < values.length; i += 2) {
            Node parent = queue.poll();

            if (!values[i].equals("-1")) {
                parent.left = new Node(Integer.parseInt(values[i]));
                queue.offer(parent.left);
            }

            if (i + 1 < values.length && !values[i + 1].equals("-1")) {
                parent.right = new Node(Integer.parseInt(values[i + 1]));
                queue.offer(parent.right);
            }
        }

        return root;
    }
}

```

```
Input
4
9
2 1 3 -1 -1 -1 5 4 7
12
6 2 9 1 5 8 -1 -1 -1 4 -1 7
12
7 3 12 1 5 11 -1 -1 -1 4 -1 10
4
28 14 -1 11
Expected Output
2 1 3 5 4 7
6 2 9 1 5 8 4 7
7 3 12 1 5 11 4 10
28 14 11
```
