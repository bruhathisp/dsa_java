Here are the solutions for the given LeetCode questions using the Fast and Slow pointers technique in Java, along with links to the problems on LeetCode:

https://leetcode.com/problems/linked-list-cycle/submissions/1309562472/


 
 






### 1. LinkedList Cycle (easy)
#### Solution:
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
}
```
#### LeetCode Link:
[LinkedList Cycle](https://leetcode.com/problems/linked-list-cycle/submissions/1309562472/)

### 2. Middle of the LinkedList (easy)
#### Solution:
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```
#### LeetCode Link:
[Middle of the LinkedList](https://leetcode.com/problems/middle-of-the-linked-list/submissions/1309563264/)

### 3. Start of LinkedList Cycle (medium)
#### Solution:
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```
#### LeetCode Link:
[Start of LinkedList Cycle](https://leetcode.com/problems/linked-list-cycle-ii/submissions/1309564490/)

### 4. Happy Number (medium)
#### Solution:
```java
class Solution {
    private int getNext(int n) {
        int totalSum = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            totalSum += d * d;
        }
        return totalSum;
    }

    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNext(n);
        while (fast != 1 && slow != fast) {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        }
        return fast == 1;
    }
}
```
#### LeetCode Link:
[Happy Number](https://leetcode.com/problems/happy-number/submissions/1309566570/)

### 5. Problem Challenge 1: Palindrome LinkedList (medium)
#### Solution:
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }

        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        ListNode prev = null, curr = slow;
        while (curr != null) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }

        ListNode left = head, right = prev;
        while (right != null) {
            if (left.val != right.val) {
                return false;
            }
            left = left.next;
            right = right.next;
        }
        return true;
    }
}
```
#### LeetCode Link:
[Palindrome LinkedList](https://leetcode.com/problems/palindrome-linked-list/description/)

### 6. Problem Challenge 2: Rearrange a LinkedList (medium)
#### Solution:
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }

        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        ListNode prev = null, curr = slow.next;
        slow.next = null;
        while (curr != null) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }

        ListNode first = head, second = prev;
        while (second != null) {
            ListNode tmp1 = first.next, tmp2 = second.next;
            first.next = second;
            second.next = tmp1;
            first = tmp1;
            second = tmp2;
        }
    }
}
```
#### LeetCode Link:
[Rearrange a LinkedList](https://leetcode.com/problems/reorder-list/submissions/1309568365/)

### 7. Problem Challenge 3: Cycle in a Circular Array (hard)
#### Solution:
```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        if (nums.length < 2) {
            return false;
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                continue;
            }

            int slow = i, fast = getNextIndex(nums, i);
            while (nums[fast] * nums[i] > 0 && nums[getNextIndex(nums, fast)] * nums[i] > 0) {
                if (slow == fast) {
                    if (slow == getNextIndex(nums, slow)) {
                        break;
                    }
                    return true;
                }
                slow = getNextIndex(nums, slow);
                fast = getNextIndex(nums, getNextIndex(nums, fast));
            }

            slow = i;
            int val = nums[i];
            while (nums[slow] * val > 0) {
                int next = getNextIndex(nums, slow);
                nums[slow] = 0;
                slow = next;
            }
        }
        return false;
    }

    private int getNextIndex(int[] nums, int i) {
        int n = nums.length;
        return ((i + nums[i]) % n + n) % n;
    }
}
```
#### LeetCode Link:
[Cycle in a Circular Array](https://leetcode.com/problems/circular-array-loop/submissions/1309568634/)

These solutions should help you solve the problems using the Fast and Slow pointers technique in Java.
