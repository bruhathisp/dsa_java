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
