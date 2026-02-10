Remember this simple rule:

## Insertion for HEAP(Both min and max)  (Upheap / Bubble Up)


Remember: 
1. Odd index = Left child
2. Even index = Right child

#### aka

For a node at index i:
- Left child index = 2*i + 1  (ALWAYS)
- Right child index = 2*i + 2  (ALWAYS)

Parent = (index - 1) / 2 (integer division)


Steps:

1. Insert at end of array
2. Compare with parent
3. If order breaks → swap
4. Repeat until correct

Time: O(log n)
(because tree height = log n)


## 🎯 Super Simplified Version (Even Easier to Memorize)

If you want separate classes (easier to remember for interviews):

```java
// ========== MIN-HEAP ==========
class MinHeap {
    private int[] heap;
    private int size;
    
    public void insert(int value) {
        heap[size] = value;
        int i = size;
        size++;
        
        // BUBBLE UP: while child < parent
        while (i > 0 && heap[i] < heap[(i-1)/2]) {
            swap(i, (i-1)/2);
            i = (i-1)/2;
        }
    }
    
    public int extractMin() {
        int min = heap[0];
        heap[0] = heap[size-1];
        size--;
        int i = 0;
        
        // BUBBLE DOWN: swap with SMALLER child
        while (true) {
            int left = 2*i + 1;
            int right = 2*i + 2;
            int smallest = i;
            
            if (left < size && heap[left] < heap[smallest]) 
                smallest = left;
            if (right < size && heap[right] < heap[smallest]) 
                smallest = right;
                
            if (smallest == i) break;
            swap(i, smallest);
            i = smallest;
        }
        return min;
    }
}
```

```java
// ========== MAX-HEAP ==========
class MaxHeap {
    private int[] heap;
    private int size;
    
    public void insert(int value) {
        heap[size] = value;
        int i = size;
        size++;
        
        // BUBBLE UP: while child > parent  
        while (i > 0 && heap[i] > heap[(i-1)/2]) {
            swap(i, (i-1)/2);
            i = (i-1)/2;
        }
    }
    
    public int extractMax() {
        int max = heap[0];
        heap[0] = heap[size-1];
        size--;
        int i = 0;
        
        // BUBBLE DOWN: swap with LARGER child
        while (true) {
            int left = 2*i + 1;
            int right = 2*i + 2;
            int largest = i;
            
            if (left < size && heap[left] > heap[largest]) 
                largest = left;
            if (right < size && heap[right] > heap[largest]) 
                largest = right;
                
            if (largest == i) break;
            swap(i, largest);
            i = largest;
        }
        return max;
    }
}
```

## 📝 Memory Cheat Sheet

### **INSERTION (bubbleUp)**
```
Min-Heap: while (child < parent) → swap
Max-Heap: while (child > parent) → swap

Parent index = (childIndex - 1) / 2
```

### **DELETION (bubbleDown)**
```
Min-Heap: swap with SMALLER child
Max-Heap: swap with LARGER child

Children indices: 
left = 2*parent + 1
right = 2*parent + 2
```


## Merge K Sorted lists using Min heap


Ref: https://www.youtube.com/watch?v=1zktEppsdig (instead of using <int, node> we just do a.val-b.val in Priority Queue)

``` java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>(
        (a, b) -> a.val - b.val // Compare node values
    );
    
    // Add first node of each list
    for (ListNode node : lists) {
        if (node != null) pq.add(node);
    }
    
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (!pq.isEmpty()) {
        ListNode smallest = pq.poll();
        current.next = smallest;
        current = current.next;
        
        if (smallest.next != null) {
            pq.add(smallest.next);
        }
    }
    return dummy.next;
}

```
