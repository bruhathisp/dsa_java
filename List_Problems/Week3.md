### 1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

**Problem Statement:** Given the head of a singly linked list, reverse the list, and return the reversed list.

**Approach:**
Initialize three pointers: `prev` as `null`, `current` as `head`, and `next` as `null`. Iterate through the list, setting `next` to `current.next`, then reversing the link by assigning `current.next` to `prev`. Move `prev` and `current` one step forward. Continue until `current` becomes `null`. Finally, return `prev` as the new head of the reversed list.

**Time Complexity:** O(n)

**Space Complexity:** O(1)

**Java Code:**

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    while (current != null) {
        ListNode next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    return prev;
}
```

### 2. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

**Problem Statement:** Given the head of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

**Approach:**
Use two pointers, `slow` and `fast`, both starting at `head`. Move `slow` one step and `fast` two steps at a time. When `fast` reaches the end of the list, `slow` will be at the middle node. Return `slow`.

**Time Complexity:** O(n)

**Space Complexity:** O(1)

**Java Code:**

```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

### 3. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

**Problem Statement:** Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

**Approach:**
Create a dummy node to serve as the start of the merged list. Use a pointer `current` to track the end of the merged list. Compare the nodes of both lists, appending the smaller node to `current` and advancing the corresponding list pointer. Continue until one list is exhausted, then append the remaining nodes of the other list. Return `dummy.next`.

**Time Complexity:** O(n + m)

**Space Complexity:** O(1)

**Java Code:**

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(-1);
    ListNode current = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }
    current.next = (list1 != null) ? list1 : list2;
    return dummy.next;
}
```


### 4. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

**Problem Statement:** Given the head of a singly linked list, determine if it is a palindrome.

**Approach:**
Find the middle of the list using the slow and fast pointer technique. Reverse the second half of the list using the reverse linked list algorithm. Compare the nodes in the first half with the nodes in the reversed second half. If all corresponding nodes are equal, the list is a palindrome. Finally, restore the list to its original order by reversing the second half again.

**Time Complexity:** O(n)

**Space Complexity:** O(1)

**Java Code:**

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }
    
    // Find the middle of the list
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse the second half of the list
    ListNode secondHalf = reverseList(slow);
    ListNode firstHalf = head;
    
    // Compare the two halves
    ListNode temp = secondHalf;
    while (temp != null) {
        if (temp.val != firstHalf.val) {
            return false;
        }
        temp = temp.next;
        firstHalf = firstHalf.next;
    }
    
    // Restore the list to its original order
    reverseList(secondHalf);
    return true;
}

// Helper method to reverse a linked list
private ListNode reverseList(ListNode head) {
    ListNode prev = null;
    while (head != null) {
        ListNode next = head.next;
        head.next = prev;
        prev = head;
        head = next;
    }
    return prev;
}
```


### 1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

**Problem Statement:** Given a linked list, determine if it has a cycle in it. To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where the tail connects back to. If `pos` is `-1`, then there is no cycle in the linked list.

**Approach:**
Utilize two pointers, `slow` and `fast`. Initialize both at the head. Move `slow` one step and `fast` two steps in each iteration. If there is a cycle, `fast` will eventually meet `slow`; if `fast` reaches the end (`null`), the list has no cycle.

**Time Complexity:** O(n)

**Space Complexity:** O(1)

**Java Code:**

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) return false;
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

### 2. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**Problem Statement:** Given the head of a linked list, remove the n-th node from the end of the list and return its head.

**Approach:**
Introduce a dummy node before the head to handle edge cases. Use two pointers, `first` and `second`, both starting at the dummy node. Move `first` n+1 steps ahead, then move both pointers simultaneously until `first` reaches the end. `Second` will be just before the target node; adjust its `next` pointer to skip the target node.

**Time Complexity:** O(L) where L is the length of the list.

**Space Complexity:** O(1)

**Java Code:**

```java
public class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode first = dummy;
        ListNode second = dummy;
        for (int i = 0; i <= n; i++) {
            first = first.next;
        }
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        second.next = second.next.next;
        return dummy.next;
    }
}
```

### 3. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

**Problem Statement:** Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection, return `null`.

**Approach:**
Use two pointers, `pA` and `pB`, starting at `headA` and `headB` respectively. Traverse each list; when a pointer reaches the end, redirect it to the head of the other list. If the lists intersect, the pointers will meet at the intersection node after at most two passes; if not, both will eventually be `null`.

**Time Complexity:** O(m + n) where m and n are the lengths of the two lists.

**Space Complexity:** O(1)

**Java Code:**

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        ListNode pA = headA;
        ListNode pB = headB;
        while (pA != pB) {
            pA = (pA == null) ? headB : pA.next;
            pB = (pB == null) ? headA : pB.next;
        }
        return pA;
    }
}
```

### 4. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

**Problem Statement:** You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

**Approach:**
Initialize a dummy node to form the result list and a `carry` variable to handle sums exceeding 9. Traverse both lists, adding corresponding digits along with the `carry`. Create new nodes for each digit of the result and update the `carry` accordingly. Continue until all digits and carry are processed.

**Time Complexity:** O(max(m, n)) where m and n are the lengths of the two lists.

**Space Complexity:** O(max(m, n)) for the result list.

**Java Code:**

```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            current.next = new ListNode(sum % 10);
            current = current.next;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry > 0) {
            current.next = new ListNode(carry);
        }
        return dummy.next;
    }
}
``` 


# Day 3

### 1. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

**Problem Statement:** Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed).

**Approach:**
1. **Dummy Node Initialization:** Create a dummy node that points to the head of the list to simplify edge cases.
2. **Pointer Setup:** Use a pointer to traverse the list, checking pairs of nodes to swap.
3. **Swapping Nodes:** For each pair, adjust the `next` pointers to swap the nodes.
4. **Iteration:** Move the pointer two nodes ahead to process the next pair.

**Time Complexity:** O(n), where n is the number of nodes in the linked list.

**Space Complexity:** O(1), as only a few extra pointers are used.

**Java Code:**

```java
public class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode current = dummy;
        
        while (current.next != null && current.next.next != null) {
            ListNode first = current.next;
            ListNode second = current.next.next;
            
            first.next = second.next;
            second.next = first;
            current.next = second;
            
            current = first;
        }
        
        return dummy.next;
    }
}
```

### 2. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

**Problem Statement:** Write a function to delete a node in a singly-linked list. You will not be given access to the head of the list; instead, you will be given access to the node to be deleted directly. It is guaranteed that the node to be deleted is not a tail node in the list.

**Approach:**
1. **Copy Value:** Copy the value of the next node into the current node.
2. **Skip Node:** Adjust the `next` pointer of the current node to skip the next node, effectively deleting it.

**Time Complexity:** O(1), as the operation involves a constant number of steps.

**Space Complexity:** O(1), as no additional space is used.

**Java Code:**

```java
public class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

### 3. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

**Problem Statement:** Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list. k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then the remaining nodes, in the end, should remain as they are.

**Approach:**
1. **Count Nodes:** Traverse the list to count the total number of nodes.
2. **Reverse in Groups:** For every group of k nodes, reverse the nodes.
3. **Handle Remaining Nodes:** If the number of remaining nodes is less than k, leave them as is.

**Time Complexity:** O(n), where n is the number of nodes in the linked list.

**Space Complexity:** O(1), as the reversal is done in place.

**Java Code:**

```java
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode current = dummy, next = dummy, prev = dummy;
        int count = 0;
        
        // Count the number of nodes in the list
        while (current.next != null) {
            current = current.next;
            count++;
        }
        
        // Reverse in groups of k
        while (count >= k) {
            current = prev.next;
            next = current.next;
            for (int i = 1; i < k; i++) {
                current.next = next.next;
                next.next = prev.next;
                prev.next = next;
                next = current.next;
            }
            prev = current;
            count -= k;
        }
        
        return dummy.next;
    }
}
```

### 4. [Rotate List](https://leetcode.com/problems/rotate-list/)

**Problem Statement:** Given the head of a linked list, rotate the list to the right by k places.

**Approach:**
1. **Compute Length:** Determine the length of the list.
2. **Connect Tail to Head:** Form a circular list by connecting the tail to the head.
3. **Find New Tail and Head:** Calculate the new tail position and break the circle to form the rotated list.

**Time Complexity:** O(n), where n is the number of nodes in the linked list.

**Space Complexity:** O(1), as the rotation is done in place.

**Java Code:**

```java
public class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;
        
        // Compute the length of the list
        ListNode current = head;
        int length = 1;
        while (current.next != null) {
            current = current.next;
            length++;
        }
        
        // Connect the tail to the head to form a circle
        current.next = head;
        
        // Find the new tail: (length - k % length - 1)th node
        // and the new head: (length - k % length)th node
        k = k % length;
        for (int i = 0; i < length - k; i++) {
            current = current.next;
        }
        
        // Break the circle
        head = current.next;
        current.next = null;
        
        return head;
    }
}
``` 
