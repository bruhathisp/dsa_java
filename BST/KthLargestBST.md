## Solution
Steps:

1. Create count array of length 1 for pass by reference
2. Recur on the right subtree `int right = findKthLargestUtil(node.right, k, count);`
3. If the kth largest element is found in the right subtree, return it
4. Increment the count for the current node
5. If the count reaches k, return the current node's value
6.  Recur on the left subtree

   
``` java
class Solution {
    // Definition for a binary tree node.
//     public static class Node {
//         public Node left;
//         public Node right;
//         public int data;

//         public Node(int data) {
//             this.data = data;
//         }
//     }

    int findKthLargest(Node root, int k) {
        int[] count = { 0 }; // Using an array to simulate pass by reference for count
        return findKthLargestUtil(root, k, count);
    }

    int findKthLargestUtil(Node node, int k, int[] count) {
        // Base case: an empty tree
        if (node == null) {
            return -1; // Invalid value, indicating no kth largest element
        }

        // Recur on the right subtree
        int right = findKthLargestUtil(node.right, k, count);

        // If the kth largest element is found in the right subtree, return it
        if (right != -1) {
            return right;
        }

        // Increment the count for the current node
        count[0]++;

        // If the count reaches k, return the current node's value
        if (count[0] == k) {
            return node.data;
        }

        // Recur on the left subtree
        return findKthLargestUtil(node.left, k, count);
    }

    
}

```
