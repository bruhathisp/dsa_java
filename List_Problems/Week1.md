# Problems 

- [Day1](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-1)
- [Day2](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-2)
- [Day3](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-3)
- [Day4](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-4)
- [Day5](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-5)
- - [Day6](https://github.com/bruhathisp/dsa_java/blob/main/List_Problems/Week1.md#day-6)



# Day 1


### 1. **Remove Duplicates from Sorted Array**
   - **LeetCode Problem**: [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
   - **Problem Breakdown**: You are given a sorted array `nums`. You need to remove duplicates in-place such that each unique element appears only once, and return the new length.
   - **Time Complexity**: O(n) (where n is the length of the array)
   - **Space Complexity**: O(1) (in-place modification)
   - **Java Solution**:
     ```java
     class Solution {
         public int removeDuplicates(int[] nums) {
             if (nums.length == 0) return 0;
             int i = 0;
             for (int j = 1; j < nums.length; j++) {
                 if (nums[j] != nums[i]) {
                     i++;
                     nums[i] = nums[j];
                 }
             }
             return i + 1;
         }
     }
     ```
   - **Explanation**: This solution uses two pointers, `i` to track the position of the last unique element and `j` to explore the array. If a new unique element is found, it is placed at the `i+1` position.

### 2. **Find the Duplicate Number** (Binary Search Approach)
   - **LeetCode Problem**: [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
   - **Approach**: Binary Search on Counts
     This solution leverages the fact that if you count how many numbers are less than or equal to a certain value, it gives clues about where the duplicate lies.
   
   - **Time Complexity**: O(n log n)
   - **Space Complexity**: O(1)
   
   - **Java Solution**:
     ```java
     class Solution {
         public int findDuplicate(int[] nums) {
             int left = 1, right = nums.length - 1;
             
             while (left < right) {
                 int mid = left + (right - left) / 2;
                 int count = 0;
                 
                 // Count numbers less than or equal to mid
                 for (int num : nums) {
                     if (num <= mid) {
                         count++;
                     }
                 }
                 
                 // If count is greater than mid, the duplicate must be in the lower half
                 if (count > mid) {
                     right = mid;
                 } else {
                     left = mid + 1;
                 }
             }
             
             return left;
         }
     }
     ```
   - **Explanation**:
     - The algorithm applies **binary search** on the range of numbers from `1` to `n` (not the array indices). 
     - For each midpoint `mid`, it counts how many numbers in the array are less than or equal to `mid`.
     - If there are more numbers than expected (i.e., `count > mid`), this indicates the duplicate lies in the range `[left, mid]`.
     - Otherwise, the duplicate is in the range `[mid + 1, right]`.
     - The search continues until the range narrows to a single number, which is the duplicate.

This binary search solution is efficient because it doesn’t rely on modifying the array or using additional space beyond simple variables.


### 3. **Maximum Subarray**
   - **LeetCode Problem**: [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
   - **Problem Breakdown**: Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public int maxSubArray(int[] nums) {
             int maxSum = nums[0];
             int currentSum = nums[0];
             
             for (int i = 1; i < nums.length; i++) {
                 currentSum = Math.max(nums[i], currentSum + nums[i]);
                 maxSum = Math.max(maxSum, currentSum);
             }
             
             return maxSum;
         }
     }
     ```
   - **Explanation**: This uses Kadane’s Algorithm, which keeps track of the maximum subarray sum ending at each index. At each step, it either starts a new subarray or adds to the existing one.

### 4. **Contains Duplicate**
   - **LeetCode Problem**: [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
   - **Problem Breakdown**: Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(n)
   - **Java Solution**:
     ```java
     import java.util.HashSet;

     class Solution {
         public boolean containsDuplicate(int[] nums) {
             HashSet<Integer> set = new HashSet<>();
             for (int num : nums) {
                 if (!set.add(num)) {
                     return true;
                 }
             }
             return false;
         }
     }
     ```
   - **Explanation**: This solution uses a hash set to track unique elements as it iterates through the array. If a duplicate is found, it returns `true` immediately.


# Day 2


### 1. **Set Matrix Zeroes**
   - **LeetCode Problem**: [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
   - **Problem Breakdown**: Given an `m x n` matrix, if an element is 0, set its entire row and column to 0. This should be done **in-place**.
   - **Time Complexity**: O(m * n) (where m is the number of rows and n is the number of columns)
   - **Space Complexity**: O(1) (constant space excluding input matrix)
   - **Java Solution**:
     ```java
     class Solution {
         public void setZeroes(int[][] matrix) {
             boolean firstRowZero = false, firstColZero = false;
             int rows = matrix.length, cols = matrix[0].length;
             
             // Check if the first row and first column need to be zeroed
             for (int i = 0; i < rows; i++) {
                 if (matrix[i][0] == 0) firstColZero = true;
             }
             for (int j = 0; j < cols; j++) {
                 if (matrix[0][j] == 0) firstRowZero = true;
             }
             
             // Use the first row and column as markers
             for (int i = 1; i < rows; i++) {
                 for (int j = 1; j < cols; j++) {
                     if (matrix[i][j] == 0) {
                         matrix[i][0] = 0;
                         matrix[0][j] = 0;
                     }
                 }
             }
             
             // Zero out cells based on markers
             for (int i = 1; i < rows; i++) {
                 for (int j = 1; j < cols; j++) {
                     if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                         matrix[i][j] = 0;
                     }
                 }
             }
             
             // Handle the first row and column separately
             if (firstColZero) {
                 for (int i = 0; i < rows; i++) {
                     matrix[i][0] = 0;
                 }
             }
             if (firstRowZero) {
                 for (int j = 0; j < cols; j++) {
                     matrix[0][j] = 0;
                 }
             }
         }
     }
     ```
   - **Explanation**: This solution uses the first row and column as markers for zeroing the entire row or column. It checks and handles the first row and column separately to avoid overwriting their information prematurely.

### 2. **Spiral Matrix**
   - **LeetCode Problem**: [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
   - **Problem Breakdown**: Given an `m x n` matrix, return all elements of the matrix in spiral order.
   - **Time Complexity**: O(m * n)
   - **Space Complexity**: O(1) (not counting the output array)
   - **Java Solution**:
     ```java
     import java.util.ArrayList;
     import java.util.List;

     class Solution {
         public List<Integer> spiralOrder(int[][] matrix) {
             List<Integer> result = new ArrayList<>();
             if (matrix.length == 0) return result;

             int top = 0, bottom = matrix.length - 1;
             int left = 0, right = matrix[0].length - 1;

             while (top <= bottom && left <= right) {
                 // Traverse from left to right across the top row
                 for (int j = left; j <= right; j++) {
                     result.add(matrix[top][j]);
                 }
                 top++;

                 // Traverse from top to bottom down the right column
                 for (int i = top; i <= bottom; i++) {
                     result.add(matrix[i][right]);
                 }
                 right--;

                 if (top <= bottom) {
                     // Traverse from right to left across the bottom row
                     for (int j = right; j >= left; j--) {
                         result.add(matrix[bottom][j]);
                     }
                     bottom--;
                 }

                 if (left <= right) {
                     // Traverse from bottom to top up the left column
                     for (int i = bottom; i >= top; i--) {
                         result.add(matrix[i][left]);
                     }
                     left++;
                 }
             }

             return result;
         }
     }
     ```
   - **Explanation**: The matrix is traversed layer by layer in a spiral order, updating the boundaries (`top`, `bottom`, `left`, `right`) as each layer is processed.

---

### 3. **Rotate Image**
   - **LeetCode Problem**: [Rotate Image](https://leetcode.com/problems/rotate-image/)
   - **Problem Breakdown**: Given an `n x n` 2D matrix representing an image, rotate the image by 90 degrees (clockwise) **in-place**.
   - **Time Complexity**: O(n^2)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public void rotate(int[][] matrix) {
             int n = matrix.length;
             
             // Transpose the matrix (swap elements across the diagonal)
             for (int i = 0; i < n; i++) {
                 for (int j = i; j < n; j++) {
                     int temp = matrix[i][j];
                     matrix[i][j] = matrix[j][i];
                     matrix[j][i] = temp;
                 }
             }
             
             // Reverse each row
             for (int i = 0; i < n; i++) {
                 for (int j = 0; j < n / 2; j++) {
                     int temp = matrix[i][j];
                     matrix[i][j] = matrix[i][n - j - 1];
                     matrix[i][n - j - 1] = temp;
                 }
             }
         }
     }
     ```
   - **Explanation**: The matrix is rotated by first transposing it (swapping across the diagonal) and then reversing each row to achieve a 90-degree clockwise rotation.

### 4. **Search a 2D Matrix**
   - **LeetCode Problem**: [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
   - **Problem Breakdown**: Given an `m x n` matrix where each row is sorted, and the first integer of each row is greater than the last integer of the previous row, write an efficient algorithm to search for a target value.
   - **Time Complexity**: O(log(m * n)) (binary search on a 1D view of the matrix)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public boolean searchMatrix(int[][] matrix, int target) {
             if (matrix.length == 0 || matrix[0].length == 0) return false;
             
             int rows = matrix.length, cols = matrix[0].length;
             int left = 0, right = rows * cols - 1;

             // Perform binary search as if the matrix is a 1D array
             while (left <= right) {
                 int mid = left + (right - left) / 2;
                 int midValue = matrix[mid / cols][mid % cols];
                 
                 if (midValue == target) {
                     return true;
                 } else if (midValue < target) {
                     left = mid + 1;
                 } else {
                     right = mid - 1;
                 }
             }
             
             return false;
         }
     }
     ```
   - **Explanation**: The matrix is treated as a 1D array for the purposes of binary search. Index `mid` is mapped to a 2D position using `mid / cols` (row index) and `mid % cols` (column index).

# Day 3

Here are the relevant LeetCode problems and their solutions for **Day 3: Strings - Basic Operations**:

---

### 1. **Valid Palindrome**
   - **LeetCode Problem**: [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
   - **Problem Breakdown**: Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
   - **Time Complexity**: O(n) (where n is the length of the string)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public boolean isPalindrome(String s) {
             int left = 0, right = s.length() - 1;
             
             while (left < right) {
                 // Move left pointer to the next alphanumeric character
                 while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                     left++;
                 }
                 // Move right pointer to the next alphanumeric character
                 while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                     right--;
                 }
                 
                 if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                     return false;
                 }
                 left++;
                 right--;
             }
             
             return true;
         }
     }
     ```
   - **Explanation**: This solution uses two pointers to compare characters from both ends of the string, ignoring non-alphanumeric characters and ignoring case. It continues until the pointers meet in the middle or a mismatch is found.

---

### 2. **Longest Common Prefix**
   - **LeetCode Problem**: [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
   - **Problem Breakdown**: Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string.
   - **Time Complexity**: O(n * m) (where n is the number of strings and m is the length of the shortest string)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public String longestCommonPrefix(String[] strs) {
             if (strs == null || strs.length == 0) return "";
             
             String prefix = strs[0];  // Assume the first string is the prefix
             
             for (int i = 1; i < strs.length; i++) {
                 while (strs[i].indexOf(prefix) != 0) {
                     prefix = prefix.substring(0, prefix.length() - 1);
                     if (prefix.isEmpty()) return "";
                 }
             }
             
             return prefix;
         }
     }
     ```
   - **Explanation**: This solution starts by assuming the first string is the longest common prefix. It then checks subsequent strings, progressively shortening the prefix until it matches the beginning of each string. If no common prefix is found, it returns an empty string.

---

### 3. **Longest Substring Without Repeating Characters**
   - **LeetCode Problem**: [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
   - **Problem Breakdown**: Given a string `s`, find the length of the longest substring without repeating characters.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(min(n, m)) (where n is the length of the string and m is the character set size, typically 128 for ASCII)
   - **Java Solution**:
     ```java
     import java.util.HashSet;

     class Solution {
         public int lengthOfLongestSubstring(String s) {
             HashSet<Character> set = new HashSet<>();
             int left = 0, maxLength = 0;
             
             for (int right = 0; right < s.length(); right++) {
                 while (set.contains(s.charAt(right))) {
                     set.remove(s.charAt(left));
                     left++;
                 }
                 set.add(s.charAt(right));
                 maxLength = Math.max(maxLength, right - left + 1);
             }
             
             return maxLength;
         }
     }
     ```
   - **Explanation**: This solution uses the sliding window technique. A hash set keeps track of the characters in the current window. If a duplicate character is found, the window is shrunk from the left until the duplicate is removed. The length of the longest valid substring is tracked during the process.

---

### 4. **Longest Palindromic Substring**
   - **LeetCode Problem**: [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
   - **Problem Breakdown**: Given a string `s`, return the longest palindromic substring in `s`.
   - **Time Complexity**: O(n^2)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         private int expandAroundCenter(String s, int left, int right) {
             while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
                 left--;
                 right++;
             }
             return right - left - 1;
         }

         public String longestPalindrome(String s) {
             if (s == null || s.length() < 1) return "";
             
             int start = 0, end = 0;
             for (int i = 0; i < s.length(); i++) {
                 int len1 = expandAroundCenter(s, i, i);  // Odd-length palindrome
                 int len2 = expandAroundCenter(s, i, i + 1);  // Even-length palindrome
                 int len = Math.max(len1, len2);
                 if (len > end - start) {
                     start = i - (len - 1) / 2;
                     end = i + len / 2;
                 }
             }
             
             return s.substring(start, end + 1);
         }
     }
     ```
   - **Explanation**: This solution expands around every possible center in the string (both single character centers and two-character centers) to check for palindromes. It tracks the start and end indices of the longest palindrome found during this expansion process.

---

# Day 4 



---

### 1. **Minimum Window Substring**
   - **LeetCode Problem**: [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
   - **Problem Breakdown**: Given two strings `s` and `t`, return the minimum window in `s` that contains all the characters in `t`. If no such window exists, return an empty string.
   - **Time Complexity**: O(n) (where n is the length of `s`)
   - **Space Complexity**: O(t) (where t is the length of string `t` for storing its character counts)
Sure! Here's a simple explanation of that `while` loop:

### What's happening:

1. **Condition**: `while (count == mapT.size())`
   - This checks if the current window has all the characters from `t` with the right frequencies. When this condition is true, it means we found a valid window.

2. **Minimize the window**:
   - We check if the current window size (`right - left`) is smaller than the smallest window we've seen so far (`minLen`).
   - If it is, we update `minLen` to this smaller window size and store the starting index (`minStart`).

3. **Shrink the window**:
   - Now we try to move the `left` pointer to shrink the window while still keeping it valid (i.e., containing all characters from `t`).
   - We look at the character at the `left` pointer (`leftChar`).

4. **Update the window**:
   - If `leftChar` is part of `t` (i.e., exists in `mapT`), we reduce its frequency in the `window` map.
   - If the frequency of `leftChar` in the window goes below the required frequency in `mapT`, we decrease `count` because the window is no longer valid.

5. **Move the `left` pointer**:
   - We increment `left` to shrink the window and continue the process until the window is no longer valid (i.e., `count < mapT.size()`).

### In short:
While the window is valid (has all characters of `t`), we check if it's the smallest one, then shrink the window from the left. If we remove a necessary character, we make the window invalid and stop shrinking.

   - **Java Solution**:
     ```java
     import java.util.HashMap;

     class Solution {
         public String minWindow(String s, String t) {
             if (s == null || t == null || s.length() < t.length()) return "";
             
             HashMap<Character, Integer> mapT = new HashMap<>();
             for (char c : t.toCharArray()) {
                 mapT.put(c, mapT.getOrDefault(c, 0) + 1);
             }
             
             int left = 0, right = 0, minStart = 0, minLen = Integer.MAX_VALUE, count = 0;
             HashMap<Character, Integer> window = new HashMap<>();
             // increment right start
             while (right < s.length()) {  
                 char rightChar = s.charAt(right);
                 if (mapT.containsKey(rightChar)) {
                     window.put(rightChar, window.getOrDefault(rightChar, 0) + 1);
                     if (window.get(rightChar).equals(mapT.get(rightChar))) {
                         count++;
                     }
                 }
                 right++;
     // increment left start
                 while (count == mapT.size()) {
                     if (right - left < minLen) {
                         minLen = right - left;
                         minStart = left;
                     }
     
                     
                     char leftChar = s.charAt(left);
     // when you remove the left character if the character in mapT, decrease the count and decrement the leftchar value from window
                     if (mapT.containsKey(leftChar)) {
                         window.put(leftChar, window.get(leftChar) - 1);
                         if (window.get(leftChar) < mapT.get(leftChar)) {
                             count--;
                         }
                     }
                     left++;
                 }
     // increment left end
             }
     // increment right end
             
             return minLen == Integer.MAX_VALUE ? "" : s.substring(minStart, minStart + minLen);
         }
     }
     ```
   - **Explanation**: This solution uses the sliding window technique to expand the right pointer until a valid window is found, then contracts the left pointer to minimize the window size while still containing all the characters from `t`.

---

### 2. **Longest Substring with At Most Two Distinct Characters**
   - **LeetCode Problem**: [Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
   - **Problem Breakdown**: Given a string `s`, find the length of the longest substring that contains at most two distinct characters.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(1) (since we're dealing with a fixed number of distinct characters)
   - **Java Solution**:
     ```java
     import java.util.HashMap;

     class Solution {
         public int lengthOfLongestSubstringTwoDistinct(String s) {
             if (s.length() < 3) return s.length();
             
             HashMap<Character, Integer> map = new HashMap<>();
             int left = 0, right = 0, maxLen = 0;
             
             while (right < s.length()) {
                 map.put(s.charAt(right), right);
                 right++;
     // When map size exceeds 2: Remove the character with the smallest index (oldest), and adjust left to shrink the window.
     //Update maxLen: After each iteration, update maxLen with the length of the current valid substring.
                 if (map.size() == 3) {
                     int deleteIndex = Integer.MAX_VALUE;
                     for (int index : map.values()) {
                         deleteIndex = Math.min(deleteIndex, index);
                     }
                     map.remove(s.charAt(deleteIndex));
                     left = deleteIndex + 1;
                 }
                 
                 maxLen = Math.max(maxLen, right - left);
             }
             
             return maxLen;
         }
     }
     ```
   - **Explanation**: This solution uses the sliding window to keep track of the last occurrence of each character. When the window contains more than two distinct characters, it removes the leftmost one and adjusts the window size.

---

### 3. **Permutation in String**
   - **LeetCode Problem**: [Permutation in String](https://leetcode.com/problems/permutation-in-string/)
   - **Problem Breakdown**: Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`.
   - **Time Complexity**: O(n) (where n is the length of `s2`)
   - **Space Complexity**: O(1) (since the character set size is fixed at 26 letters)
   - **Java Solution**:
     ```java
     class Solution {
         public boolean checkInclusion(String s1, String s2) {
             if (s1.length() > s2.length()) return false;

             int[] s1Count = new int[26];
             int[] s2Count = new int[26];

             for (int i = 0; i < s1.length(); i++) {
                 s1Count[s1.charAt(i) - 'a']++;
                 s2Count[s2.charAt(i) - 'a']++;
             }

             for (int i = 0; i < s2.length() - s1.length(); i++) {
                 if (matches(s1Count, s2Count)) return true;
                 s2Count[s2.charAt(i + s1.length()) - 'a']++;
                 s2Count[s2.charAt(i) - 'a']--;
             }

             return matches(s1Count, s2Count);
         }

         private boolean matches(int[] s1Count, int[] s2Count) {
             for (int i = 0; i < 26; i++) {
                 if (s1Count[i] != s2Count[i]) return false;
             }
             return true;
         }
     }
     ```
   - **Explanation**: This solution maintains a sliding window of the size of `s1` in `s2`, checking if the character frequencies in the window match the character frequencies in `s1`.

---

### 4. **Find All Anagrams in a String**
   - **LeetCode Problem**: [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
   - **Problem Breakdown**: Given a string `s` and a string `p`, return the start indices of all anagrams of `p` in `s`.
   - **Time Complexity**: O(n) (where n is the length of `s`)
   - **Space Complexity**: O(1) (because the character set is fixed)
   - **Java Solution**:
     ```java
     import java.util.ArrayList;
     import java.util.List;

     class Solution {
         public List<Integer> findAnagrams(String s, String p) {
             List<Integer> result = new ArrayList<>();
             if (s.length() < p.length()) return result;

             int[] pCount = new int[26];
             int[] sCount = new int[26];

             for (int i = 0; i < p.length(); i++) {
                 pCount[p.charAt(i) - 'a']++;
                 sCount[s.charAt(i) - 'a']++;
             }

             for (int i = 0; i < s.length() - p.length(); i++) {
                 if (matches(pCount, sCount)) result.add(i);
                 sCount[s.charAt(i + p.length()) - 'a']++;
                 sCount[s.charAt(i) - 'a']--;
             }

             if (matches(pCount, sCount)) result.add(s.length() - p.length());

             return result;
         }

         private boolean matches(int[] pCount, int[] sCount) {
             for (int i = 0; i < 26; i++) {
                 if (pCount[i] != sCount[i]) return false;
             }
             return true;
         }
     }
     ```
   - **Explanation**: This solution uses a sliding window of size `p.length()` and checks if the character frequencies in the current window match those in `p`. If they match, the start index of the window is added to the result list.

---


# Day 5
Here are the solutions for **Day 5: Two Pointers Technique**:

---

### 1. **3Sum**
   - **LeetCode Problem**: [3Sum](https://leetcode.com/problems/3sum/)
   - **Problem Breakdown**: Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`. The solution set must not contain duplicate triplets.
   - **Time Complexity**: O(n^2) (due to sorting and two-pointer traversal for each element)
   - **Space Complexity**: O(n) (space for the result list)
The strategy is to first sort the array `nums` and then use a for-loop with index `i` to iterate through the array, where `nums[i]` represents the first element of the triplet. For each `i`, two pointers `left` and `right` are initialized to `i + 1` and `nums.length - 1`, respectively, to find the other two elements of the triplet. The sum of `nums[i]`, `nums[left]`, and `nums[right]` is computed, and based on the sum, the pointers are adjusted: if the sum is zero, the triplet is added to the result and both pointers are moved inward, skipping duplicates. If the sum is less than zero, `left` is incremented to increase the sum, and if the sum is greater than zero, `right` is decremented to reduce the sum. This process continues until all unique triplets are found.
   - **Java Solution**:
     ```java
     import java.util.*;

     class Solution {
         public List<List<Integer>> threeSum(int[] nums) {
             List<List<Integer>> result = new ArrayList<>();
             Arrays.sort(nums);  // Sort the array to use two pointers
             
             for (int i = 0; i < nums.length - 2; i++) {
                 if (i > 0 && nums[i] == nums[i - 1]) continue;  // Skip duplicates
                 
                 int left = i + 1, right = nums.length - 1;
                 while (left < right) {
                     int sum = nums[i] + nums[left] + nums[right];
                     if (sum == 0) {
                         result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                         while (left < right && nums[left] == nums[left + 1]) left++;  // Skip duplicates
                         while (left < right && nums[right] == nums[right - 1]) right--;  // Skip duplicates
                         left++;
                         right--;
                     } else if (sum < 0) {
                         left++;
                     } else {
                         right--;
                     }
                 }
             }
             
             return result;
         }
     }
     ```
   - **Explanation**: The solution sorts the array and uses a fixed pointer (`i`) and two other pointers (`left` and `right`). It skips duplicates to avoid repeated triplets and moves the pointers based on the sum of the three numbers.

---

### 2. **Container With Most Water**
   - **LeetCode Problem**: [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
   - **Problem Breakdown**: You are given an integer array `height` representing the height of lines. Find two lines that, together with the x-axis, form a container that can hold the most water.
   - **Time Complexity**: O(n) (single traversal of the array using two pointers)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public int maxArea(int[] height) {
             int left = 0, right = height.length - 1;
             int maxArea = 0;
             
             while (left < right) {
                 int width = right - left;
                 int area = Math.min(height[left], height[right]) * width;
                 maxArea = Math.max(maxArea, area);
                 
                 if (height[left] < height[right]) {
                     left++;
                 } else {
                     right--;
                 }
             }
             
             return maxArea;
         }
     }
     ```
   - **Explanation**: This two-pointer solution starts at both ends of the array. The idea is to maximize the area by gradually narrowing the window from both sides. The shorter line between the two pointers is moved inward because it limits the area.

---

### 3. **Two Sum II**
   - **LeetCode Problem**: [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
   - **Problem Breakdown**: Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public int[] twoSum(int[] numbers, int target) {
             int left = 0, right = numbers.length - 1;
             
             while (left < right) {
                 int sum = numbers[left] + numbers[right];
                 if (sum == target) {
                     return new int[]{left + 1, right + 1};  // Return 1-indexed positions
                 } else if (sum < target) {
                     left++;
                 } else {
                     right--;
                 }
             }
             
             return new int[]{-1, -1};  // In case no solution is found
         }
     }
     ```
   - **Explanation**: The solution leverages the sorted nature of the array to use two pointers. It moves the left pointer if the sum is less than the target, or the right pointer if the sum is greater.

---

### 4. **Remove Duplicates from Sorted Array II**
   - **LeetCode Problem**: [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
   - **Problem Breakdown**: Given a sorted array `nums`, remove duplicates in-place such that duplicates appear at most twice, and return the new length.
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(1)
   - **Java Solution**:
     ```java
     class Solution {
         public int removeDuplicates(int[] nums) {
             if (nums.length <= 2) return nums.length;
             
             int i = 2;  // Start from index 2 since the first two elements are allowed
             
             for (int j = 2; j < nums.length; j++) {
                 if (nums[j] != nums[i - 2]) {
                     nums[i] = nums[j];
                     i++;
                 }
             }
             
             return i;
         }
     }
     ```
   - **Explanation**: The strategy is to modify the array in place to remove duplicates such that each element appears at most twice. The variable `i` starts at 2, assuming the first two elements can always remain in the array. The loop iterates through the array with index `j` starting at 2, and for each element `nums[j]`, it checks if it is different from `nums[i - 2]`. If it's different, it means the element can remain in the array, so `nums[i]` is set to `nums[j]`, and `i` is incremented. This process ensures that no element appears more than twice, and the function returns `i`, which represents the new length of the modified array.

---

These problems effectively demonstrate the **two pointers technique**, a powerful approach for solving problems involving arrays and strings with linear complexity. This method optimizes performance by reducing unnecessary comparisons or recalculations.


# Day 6


### 1. Subarray Sum Equals K

**Problem Statement:** Given an array of integers `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals to `k` citeturn0search2.

**Approach:**

Utilize the prefix sum technique with a hash map to store the frequency of each prefix sum. As you iterate through the array, calculate the current prefix sum and check if `(prefix_sum - k)` exists in the hash map. If it does, it indicates that there's a subarray ending at the current index with a sum of `k`.

**Time Complexity:** O(n), where n is the length of `nums`.

**Space Complexity:** O(n), due to the storage of the hash map.

**Java Code:**

```java
import java.util.HashMap;

public class SubarraySumEqualsK {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int prefixSum = 0;
        HashMap<Integer, Integer> prefixSumMap = new HashMap<>();
        prefixSumMap.put(0, 1); // Initialize with prefix sum 0

        for (int num : nums) {
            prefixSum += num;
            if (prefixSumMap.containsKey(prefixSum - k)) {
                count += prefixSumMap.get(prefixSum - k);
            }
            prefixSumMap.put(prefixSum, prefixSumMap.getOrDefault(prefixSum, 0) + 1);
        }

        return count;
    }
}
```

### 2. Product of Array Except Self

**Problem Statement:** Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`. You must write an algorithm that runs in O(n) time and without using the division operation citeturn0search1.

**Approach:**

Compute the product of all elements to the left and right of each index. Traverse the array twice: once from left to right to compute the left products, and once from right to left to compute the right products. Multiply the left and right products to get the final result.

**Time Complexity:** O(n), where n is the length of `nums`.

**Space Complexity:** O(1), as the output array does not count as extra space.

**Java Code:**

```java
public class ProductOfArrayExceptSelf {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] answer = new int[n];
        
        // Compute left products
        int left = 1;
        for (int i = 0; i < n; i++) {
            answer[i] = left;
            left *= nums[i];
        }
        
        // Compute right products and final result
        int right = 1;
        for (int i = n - 1; i >= 0; i--) {
            answer[i] *= right;
            right *= nums[i];
        }
        
        return answer;
    }
}
```

### 3. Maximum Subarray

**Problem Statement:** Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum citeturn0search3.

**Approach:**

Implement Kadane's Algorithm, which involves iterating through the array and maintaining the current subarray sum and the maximum sum found so far. If the current sum becomes negative, reset it to zero, as negative sums do not contribute to the maximum sum.

**Time Complexity:** O(n), where n is the length of `nums`.

**Space Complexity:** O(1), as it uses a constant amount of extra space.

**Java Code:**

```java
public class MaximumSubarray {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int currentSum = 0;

        for (int num : nums) {
            currentSum += num;
            if (currentSum > maxSum) {
                maxSum = currentSum;
            }
            if (currentSum < 0) {
                currentSum = 0;
            }
        }

        return maxSum;
    }
}
```

### 4. Continuous Subarray Sum

**Problem Statement:** Given an integer array `nums` and an integer `k`, return true if `nums` has a continuous subarray of size at least two whose elements sum up to a multiple of `k`, or false otherwise citeturn0search0.

**Approach:**

Use modular arithmetic to determine if the sum of any subarray is a multiple of `k`. Maintain a hash map to store the index of the first occurrence of each prefix sum modulo `k`. As you iterate through the array, calculate the current prefix sum modulo `k` and check if it has been seen before. If it has, and the subarray length is at least two, return true.

**Time Complexity:** O(n), where n is the length of `nums`.

**Space Complexity:** O(n), due to the storage of the hash map.

**Java Code:**

```java
import java.util.HashMap;

public class ContinuousSubarraySum {
    public boolean checkSubarraySum(int[] nums, int k) {
        int prefixSum = 0;
        HashMap<Integer, Integer> modIndexMap = new HashMap<>();
        modIndexMap.put(0, -1); // Initialize with modulus 0 at index -1

        for (int i = 0; i < nums.length; i++) {
            prefixSum += nums[i];
            int modulus = prefixSum % k;
            if (modIndexMap.containsKey(modulus)) {
                if (i - modIndexMap.get(modulus) >= 2) {
                    return true;
                }
            } else {
                modIndexMap.put(modulus, i);
            }
        }

        return false;
    }
}
```
