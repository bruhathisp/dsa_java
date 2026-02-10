Remember this simple rule:

Insertion for HEAP(Both min and max)


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
