# Problems

- [Day1](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week2.md#day-1)

# Day 1

### 1. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

**Problem Statement:** Given an array of strings `strs`, group the anagrams together. You can return the answer in any order. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase.

**Approach:**  
We use a hashmap where the key is the sorted version of each string, and the value is a list of anagrams corresponding to that key. For each string in the array, we sort its characters and use the sorted string as the key to group all anagrams together in the hashmap. Finally, we return all the grouped anagrams.

**Time Complexity:** O(n * k log k), where `n` is the number of strings and `k` is the maximum length of a string.

**Space Complexity:** O(nk), due to the hashmap storing all strings.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String str : strs) {
            char[] charArray = str.toCharArray();
            Arrays.sort(charArray);
            String sortedStr = new String(charArray);
            
            map.putIfAbsent(sortedStr, new ArrayList<>());
            map.get(sortedStr).add(str);
        }
        
        return new ArrayList<>(map.values());
    }
}
```

---

### 2. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

**Problem Statement:** Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence. The sequence must be consecutive numbers in any order.

**Approach:**  
We use a hash set to store all the numbers in the array. For each number, we check if it is the start of a sequence (i.e., `num - 1` is not in the set). If it is, we count the length of the sequence by incrementing the current number and checking if the next consecutive number exists in the set. We track the maximum sequence length encountered.

**Time Complexity:** O(n), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), due to the hash set storing all numbers.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }

        int longestStreak = 0;

        for (int num : numSet) {
            if (!numSet.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (numSet.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```

---

### 3. [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

**Problem Statement:** Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

**Approach:**  
We use a hashmap to track the most recent index of each element in the array. As we iterate through `nums`, for each element, we check if it already exists in the hashmap and whether the difference between the current index and the stored index is less than or equal to `k`. If so, we return `true`. If no such pair is found, we return `false`.

**Time Complexity:** O(n), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), due to the hashmap storing indices.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) {
                return true;
            }
            map.put(nums[i], i);
        }
        
        return false;
    }
}
```

---

### 4. [Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

**Problem Statement:** Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays, and you may return the result in any order.

**Approach:**  
We use a hashmap to store the count of each element in `nums1`. Then, we iterate through `nums2`, and for each element that exists in the hashmap with a non-zero count, we add it to the result and decrement the count. This ensures that we account for the intersection, including duplicates.

**Time Complexity:** O(n + m), where `n` is the length of `nums1` and `m` is the length of `nums2`.

**Space Complexity:** O(min(n, m)), for the hashmap storing the smaller array's elements.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }
        
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        List<Integer> result = new ArrayList<>();
        for (int num : nums2) {
            if (map.containsKey(num) && map.get(num) > 0) {
                result.add(num);
                map.put(num, map.get(num) - 1);
            }
        }
        
        int[] intersection = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            intersection[i] = result.get(i);
        }
        
        return intersection;
    }
}
```

# Day 2

### 1. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

**Problem Statement:** Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Approach:**  
We use a hashmap to count the frequency of each element in the array. Then, using a priority queue (min-heap) of size `k`, we keep track of the most frequent elements. For each element in the frequency map, if the heap size is less than `k`, we push the element. If the heap size equals `k`, we compare the frequency of the current element with the minimum frequency element in the heap and replace if necessary. Finally, the heap contains the `k` most frequent elements.

**Time Complexity:** O(n log k), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), due to the hashmap and heap.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<Integer> heap = new PriorityQueue<>(
            (n1, n2) -> freqMap.get(n1) - freqMap.get(n2)
        );

        for (int num : freqMap.keySet()) {
            heap.add(num);
            if (heap.size() > k) {
                heap.poll();
            }
        }

        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = heap.poll();
        }
        return result;
    }
}
```

---

### 2. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

**Problem Statement:** Given an array of integers `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals to `k`.

**Approach:**  
We use a hashmap to store the cumulative sum up to the current index. For each element, we calculate the cumulative sum. If the cumulative sum minus `k` exists in the hashmap, it means there is a subarray that sums to `k`. We also track the occurrences of each cumulative sum in the hashmap.

**Time Complexity:** O(n), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), due to the hashmap storing cumulative sums.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> sumMap = new HashMap<>();
        sumMap.put(0, 1);
        int count = 0, sum = 0;

        for (int num : nums) {
            sum += num;
            if (sumMap.containsKey(sum - k)) {
                count += sumMap.get(sum - k);
            }
            sumMap.put(sum, sumMap.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```

---

### 3. [4Sum](https://leetcode.com/problems/4sum/)

**Problem Statement:** Given an array `nums` of `n` integers and an integer `target`, return all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that `a, b, c, d` are distinct indices and the sum of the quadruplets is equal to the target.

**Approach:**  
We first sort the array, then use two nested loops to fix the first two elements and apply a two-pointer approach for the remaining two elements. After fixing the first two elements, we move the pointers based on the sum comparison with the target. To ensure unique quadruplets, we skip duplicate elements during the iteration.

**Time Complexity:** O(n³), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), for storing the result.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length < 4) return result;
        
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue; // skip duplicates
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue; // skip duplicates
                int left = j + 1, right = nums.length - 1;
                
                while (left < right) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while (left < right && nums[left] == nums[left + 1]) left++; // skip duplicates
                        while (left < right && nums[right] == nums[right - 1]) right--; // skip duplicates
                        left++;
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        
        return result;
    }
}
```

---

### 4. [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

**Problem Statement:** Given a string `s` and an integer `k`, return the length of the longest substring that contains at most `k` distinct characters.

**Approach:**  
We use a sliding window approach with two pointers. We maintain a hashmap that keeps track of the count of characters in the current window. As we expand the window by moving the right pointer, we check if the number of distinct characters exceeds `k`. If it does, we move the left pointer to reduce the number of distinct characters until it is at most `k`.

**Time Complexity:** O(n), where `n` is the length of the string `s`.

**Space Complexity:** O(k), since the hashmap stores at most `k` distinct characters.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0 || k == 0) return 0;
        
        Map<Character, Integer> charMap = new HashMap<>();
        int left = 0, maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            charMap.put(c, charMap.getOrDefault(c, 0) + 1);
            
            while (charMap.size() > k) {
                char leftChar = s.charAt(left);
                charMap.put(leftChar, charMap.get(leftChar) - 1);
                if (charMap.get(leftChar) == 0) {
                    charMap.remove(leftChar);
                }
                left++;
            }
            
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```

# Day 3

### 1. [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

**Problem Statement:** The Fibonacci numbers, commonly denoted `F(n)`, form a sequence, such that each number is the sum of the two preceding ones, starting from `0` and `1`. Given `n`, calculate `F(n)`.

**Approach:**  
We solve this problem using recursion. The base cases are when `n == 0` (return `0`) and `n == 1` (return `1`). For other values of `n`, we recursively call `fib(n - 1)` and `fib(n - 2)` and sum their results.

**Time Complexity:** O(2^n), due to the overlapping subproblems in the recursive calls.

**Space Complexity:** O(n), for the recursion stack.

**Java Code:**

```java
public class Solution {
    public int fib(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        return fib(n - 1) + fib(n - 2);
    }
}
```

---

### 2. [Reverse String](https://leetcode.com/problems/reverse-string/)

**Problem Statement:** Write a function that reverses a string. The input string is given as an array of characters `s`. You must do this by modifying the input array in place with O(1) extra memory.

**Approach:**  
We use a recursive approach to reverse the string. We use two pointers: one at the start and one at the end of the string. We swap the characters at these positions and recursively call the function for the remaining substring by moving the pointers inward.

**Time Complexity:** O(n), where `n` is the number of characters in the string.

**Space Complexity:** O(n), due to the recursion stack.

**Java Code:**

```java
public class Solution {
    public void reverseString(char[] s) {
        reverse(s, 0, s.length - 1);
    }
    
    private void reverse(char[] s, int left, int right) {
        if (left >= right) return;
        
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        
        reverse(s, left + 1, right - 1);
    }
}
```

---

### 3. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

**Problem Statement:** You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Approach:**  
This problem can be modeled as the Fibonacci sequence. If you're on the `n-th` step, you could have arrived from either `n-1` or `n-2`. Thus, `ways(n) = ways(n-1) + ways(n-2)`. We solve this using recursion with the base cases being `ways(0) = 1` and `ways(1) = 1`.

**Time Complexity:** O(2^n), due to the exponential growth of recursive calls.

**Space Complexity:** O(n), for the recursion stack.

**Java Code:**

```java
public class Solution {
    public int climbStairs(int n) {
        if (n == 0 || n == 1) return 1;
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```

---

### 4. [Power of Two](https://leetcode.com/problems/power-of-two/)

**Problem Statement:** Given an integer `n`, return `true` if it is a power of two. Otherwise, return `false`. An integer `n` is a power of two if there exists an integer `x` such that `n == 2^x`.

**Approach:**  
We recursively divide the number by 2 if it is even, and check if the resulting value is 1. If `n == 1`, return `true`, since `2^0 = 1`. If `n` is odd and greater than 1, return `false`.

**Time Complexity:** O(log n), where `n` is divided by 2 at each recursive step.

**Space Complexity:** O(log n), for the recursion stack.

**Java Code:**

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n <= 0) return false;
        if (n == 1) return true;
        if (n % 2 != 0) return false;
        return isPowerOfTwo(n / 2);
    }
}
```

# Day 4

### 1. [Subsets](https://leetcode.com/problems/subsets/)

**Problem Statement:** Given an integer array `nums` of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets, and you may return the subsets in any order.

**Approach:**  
We use backtracking to generate all subsets. Starting from an empty list, we iterate over the elements of `nums`, adding each element to the current subset. After adding an element, we recursively call the function to generate further subsets with the current subset. At each step, we backtrack by removing the last element added and continue exploring other subsets.

**Time Complexity:** O(2^n), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n), for storing the temporary subsets during recursion.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums, 0);
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> tempList, int[] nums, int start) {
        result.add(new ArrayList<>(tempList));
        for (int i = start; i < nums.length; i++) {
            tempList.add(nums[i]);
            backtrack(result, tempList, nums, i + 1);
            tempList.remove(tempList.size() - 1);  // backtrack
        }
    }
}
```

---

### 2. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

**Problem Statement:** Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in any order. A mapping of digits to letters is given, with the same mapping as on a phone keypad.

**Approach:**  
We use backtracking to generate all combinations of letters based on the input digits. For each digit, we retrieve the corresponding characters and append one character at a time to the current combination. We recursively build the combinations for the remaining digits until we exhaust the input string.

**Time Complexity:** O(4^n), where `n` is the number of digits, and each digit maps to 3 or 4 letters.

**Space Complexity:** O(n), for the recursion stack and temporary combinations.

**Java Code:**

```java
import java.util.*;

public class Solution {
    private static final String[] KEYPAD = {
        "",    "",    "abc",  "def", 
        "ghi", "jkl", "mno",  "pqrs", 
        "tuv", "wxyz"
    };
    
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (digits == null || digits.length() == 0) return result;
        backtrack(result, new StringBuilder(), digits, 0);
        return result;
    }
    
    private void backtrack(List<String> result, StringBuilder temp, String digits, int index) {
        if (index == digits.length()) {
            result.add(temp.toString());
            return;
        }
        
        String letters = KEYPAD[digits.charAt(index) - '0'];
        for (char letter : letters.toCharArray()) {
            temp.append(letter);
            backtrack(result, temp, digits, index + 1);
            temp.deleteCharAt(temp.length() - 1);  // backtrack
        }
    }
}
```

---

### 3. [Permutations](https://leetcode.com/problems/permutations/)

**Problem Statement:** Given an array `nums` of distinct integers, return all possible permutations. You can return the answer in any order.

**Approach:**  
We use backtracking to generate all permutations. Starting from an empty list, we add one element at a time from `nums` that hasn't been added yet. After adding an element, we recursively generate the next element of the permutation. Once we have a complete permutation, we add it to the result list. After each recursive call, we backtrack by removing the last added element.

**Time Complexity:** O(n!), where `n` is the number of elements in `nums`.

**Space Complexity:** O(n!), for storing all permutations.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums);
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> tempList, int[] nums) {
        if (tempList.size() == nums.length) {
            result.add(new ArrayList<>(tempList));
        } else {
            for (int i = 0; i < nums.length; i++) {
                if (tempList.contains(nums[i])) continue; // element already exists, skip
                tempList.add(nums[i]);
                backtrack(result, tempList, nums);
                tempList.remove(tempList.size() - 1);  // backtrack
            }
        }
    }
}
```

---

### 4. [Combination Sum](https://leetcode.com/problems/combination-sum/)

**Problem Statement:** Given an array of distinct integers `candidates` and a target integer `target`, return all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order. The same number may be chosen from `candidates` an unlimited number of times.

**Approach:**  
We use backtracking to explore all combinations that sum to `target`. Starting with an empty list, we iterate through `candidates` and subtract the current number from the target. We recursively call the function to explore further, allowing the same number to be reused. When the target becomes zero, we add the current combination to the result. We backtrack by removing the last added element when the target becomes negative or when we explore all options.

**Time Complexity:** O(2^n), where `n` is the number of candidates and we may have multiple recursive branches.

**Space Complexity:** O(n), for the recursion stack and temporary combinations.

**Java Code:**

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), candidates, target, 0);
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> tempList, int[] candidates, int target, int start) {
        if (target < 0) return; // exceeded target
        if (target == 0) {
            result.add(new ArrayList<>(tempList)); // valid combination
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            tempList.add(candidates[i]);
            backtrack(result, tempList, candidates, target - candidates[i], i); // not i + 1 since we can reuse elements
            tempList.remove(tempList.size() - 1);  // backtrack
        }
    }
}
```
