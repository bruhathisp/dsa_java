## Solution


Steps
Tterate through both lists, comparing the current nodes of each list. 
It selects the smaller node and adds it to the merged list. 
If one of the lists gets exhausted before the other, the remaining elements of the non-empty list are added to the end of the merged list. 
The dummy node is used as a placeholder to simplify the list construction process. The actual head of the merged list is dummy.next.
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Initialize a dummy node to build the merged list
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        // Loop through both lists while neither is empty
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }
            tail = tail.next;
        }

        // Attach the remaining elements, if any
        if (list1 != null) {
            tail.next = list1;
        } else if (list2 != null) {
            tail.next = list2;
        }

        return dummy.next; // The merged list is next to the dummy node
    }
}

```
