Here's a simple explanation of the four approaches to solve the "Contains Duplicate" problem along with the Java code for each approach:

Approach 1: Brute Force
- The brute force approach involves comparing each element in the array with every other element to check for duplicates.
- If any duplicates are found, the function returns true, otherwise, it returns false.
- This approach has a time complexity of O(n^2), which makes it inefficient for large arrays.

Java Code:
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] == nums[j])
                    return true;
            }
        }
        return false;
    }
}
```

Approach 2: Sorting
- The sorting approach sorts the array in ascending order and then checks for adjacent elements that are the same.
- If any duplicates are found, the function returns true, otherwise, it returns false.
- Sorting helps in bringing duplicates together, simplifying the check.
- This approach has a time complexity of O(n log n).

Java Code:
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 1; i < n; i++) {
            if (nums[i] == nums[i - 1])
                return true;
        }
        return false;
    }
}
```

Approach 3: Hash Set
- The hash set approach uses a HashSet data structure to store encountered elements.
- It iterates through the array, checking if an element is already in the set.
- If so, the function returns true, indicating the presence of duplicates.
- Otherwise, it adds the element to the set and continues the iteration.
- This approach has a time complexity of O(n), which provides an efficient way to check for duplicates.

Java Code:
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        for (int num : nums) {
            if (seen.contains(num))
                return true;
            seen.add(num);
        }
        return false;
    }
}
```

Approach 4: Hash Map
- The hash map approach is similar to the hash set approach but also keeps track of the count of occurrences for each element.
- It uses a HashMap to store the elements as keys and their counts as values.
- If a duplicate element is encountered (count greater than or equal to 1), the function returns true, indicating the presence of duplicates.
- Otherwise, it updates the count of that element in the hash map and continues the iteration.
- This approach also has a time complexity of O(n) and provides more information than just the presence of duplicates.

Java Code:
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int num : nums) {
            if (seen.containsKey(num) && seen.get(num) >= 1)
                return true;
            seen.put(num, seen.getOrDefault(num, 0) + 1);
        }
        return false;
    }
}
// seen.getOrDefault(num, 0): This is a method call on the HashMap seen.

// It tries to retrieve the value associated with the key num. If the key num is present in the HashMap,

// it returns its value (the count of occurrences of num).

// If the key num is not present, it returns the default value 0.

// seen.getOrDefault(num, 0) + 1: This is adding 1 to the count of occurrences of the element num.

// If num is already present in the HashMap, this will increase its count by 1. If num is not present, it will start its count from 1.

// seen.put(num, seen.getOrDefault(num, 0) + 1): Finally, we use the put method on the seen HashMap

// to update the count of occurrences of the element num. If num is already present in the HashMap,

// this will update its count to the new value. If num is not present, it will add num as a new key with a count of 1.




```

All four approaches can help determine if there are duplicates in the array, but the hash set and hash map approaches are the most efficient with a time complexity of O(n) compared to the sorting and brute force approaches.
