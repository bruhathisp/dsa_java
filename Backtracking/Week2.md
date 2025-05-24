
# 🔁 Backtracking Problems in Java

---

## 🧩 1. Subsets ([LeetCode #78](https://leetcode.com/problems/subsets/))

**Concept**: Generate all possible subsets of a given set of integers.

### 💻 Java Solution

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
        result.add(new ArrayList<>(current));
        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);
            backtrack(nums, i + 1, current, result);
            current.remove(current.size() - 1);
        }
    }
}
````

### 🧠 Thought Process

* Start with an empty subset.
* Iterate through the array, adding each element to the current subset.
* Recursively generate subsets starting from the next index.
* Backtrack by removing the last added element to explore other possibilities.
  *(Source: [redquark.org](https://redquark.org), Airtribe)*

---

## 📞 2. Letter Combinations of a Phone Number ([LeetCode #17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/))

**Concept**: Given a string representing digits from 2-9, return all possible letter combinations that the number could represent.
*(Source: Program Creek)*

### 💻 Java Solution

```java
class Solution {
    private static final String[] MAPPINGS = {
        "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (digits == null || digits.isEmpty()) return result;
        backtrack(digits, 0, new StringBuilder(), result);
        return result;
    }

    private void backtrack(String digits, int index, StringBuilder current, List<String> result) {
        if (index == digits.length()) {
            result.add(current.toString());
            return;
        }
        String letters = MAPPINGS[digits.charAt(index) - '2'];
        for (char letter : letters.toCharArray()) {
            current.append(letter);
            backtrack(digits, index + 1, current, result);
            current.deleteCharAt(current.length() - 1);
        }
    }
}
```

### 🧠 Thought Process

* Map each digit to its corresponding letters.
* Use a `StringBuilder` to build combinations.
* Recursively append each letter and backtrack to explore all combinations.
  *(Source: [redquark.org](https://redquark.org))*

---

## 🔄 3. Permutations ([LeetCode #46](https://leetcode.com/problems/permutations/))

**Concept**: Given a collection of distinct integers, return all possible permutations.
*(Source: Program Creek)*

### 💻 Java Solution

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, new boolean[nums.length], new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] nums, boolean[] used, List<Integer> current, List<List<Integer>> result) {
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            current.add(nums[i]);
            backtrack(nums, used, current, result);
            current.remove(current.size() - 1);
            used[i] = false;
        }
    }
}
```

### 🧠 Thought Process

* Maintain a boolean array to track used elements.
* Build permutations by adding unused elements.
* Backtrack by removing the last added element and marking it as unused.
  *(Source: Medium)*

---

## 🔢 4. Combination Sum ([LeetCode #39](https://leetcode.com/problems/combination-sum/))

**Concept**: Given an array of candidate numbers and a target number, find all unique combinations in the array where the candidate numbers sum to the target.

### 💻 Java Solution

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            if (candidates[i] <= target) {
                current.add(candidates[i]);
                backtrack(candidates, target - candidates[i], i, current, result);
                current.remove(current.size() - 1);
            }
        }
    }
}
```

### 🧠 Thought Process

* Iterate through the candidates starting from the current index.
* Add the candidate to the current combination and reduce the target.
* Recursively find combinations with the updated target.
* Backtrack by removing the last added element.
  *(Source: [redquark.org](https://redquark.org))*

---

## 🔍 Word Search ([LeetCode #79](https://leetcode.com/problems/word-search/))

**Concept**: Check if a word exists in a 2D grid by traversing adjacent cells (horizontally or vertically).

### 💻 Java Solution

```java
class Solution {
   public boolean exist(char[][] board, String word) {
       int rows = board.length;
       int cols = board[0].length;

       for (int r = 0; r < rows; r++) {
           for (int c = 0; c < cols; c++) {
               if (dfs(board, r, c, word, 0)) {
                   return true;
               }
           }
       }
       return false;
   }

   private boolean dfs(char[][] board, int r, int c, String word, int index) {
       // 1. Base case: full match
       if (index == word.length()) return true;

       // 2. Boundary and mismatch checks
       if (r < 0 || c < 0 || r >= board.length || c >= board[0].length
           || board[r][c] != word.charAt(index)) {
           return false;
       }

       // 3. Save the current char and mark this cell as visited
       char temp = board[r][c];
       board[r][c] = '#'; // mark as visited

       // 4. Explore all 4 directions
       boolean found = dfs(board, r + 1, c, word, index + 1) ||
                       dfs(board, r - 1, c, word, index + 1) ||
                       dfs(board, r, c + 1, word, index + 1) ||
                       dfs(board, r, c - 1, word, index + 1);

       // 5. Backtrack (restore the cell)
       board[r][c] = temp;

       return found;
   }
}
```

