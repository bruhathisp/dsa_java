## Solution


Lessons Learnt:

1. `A.substring(i, end).equals(word)`  to find the substring 


Steps:


1. dp is a array of strings with the length of A+1
2. Loop through the A (`i->A.length()`) and for every word B   `end` index of the substring being considered.:  `end = i + length;`
   1. If dp is null skip iteration (when there is no valid entry in the dp )
   2.   Update dp if a valid word is found: `A.substring(i, end).equals(word)`. Update by `dp[end].add(word);`
      1. If dp[end] is null, create a new ArrayList and add the current word. If it's not null, simply add the word to the existing ArrayList.
3. If dp is null return empty array. else call `dfs(dp, A.length(), resultSet, temp);` and have a temp variable which is an array of strings

   

### dfs

3. The termination in dfs is given below

## Iteration in dp

1. **For Each Word in `dp[end]`:**
  

2. **Add Word to Current Combination (`temp`):**
   - `temp.add(str);`: Adds the current word (`str`) to the temporary combination of words (`temp`). This is done to build a sequence of words as the algorithm explores different combinations.

3. **Recursive DFS Call:**
   - `dfs(dp, end - str.length(), result, temp);`: It moves to the next position in the input string by subtracting the length of the current word (`str`) from the current end index (`end`). This is essential for exploring different combinations by moving backward in the input string.

4. **Backtracking: Remove Last Word from Combination (`temp`):**
   - `temp.remove(temp.size() - 1);`: After the recursive call returns, the last added word (`str`) is removed from the temporary combination (`temp`). This is a form of backtracking, ensuring that the algorithm explores all possibilities and doesn't miss any valid combinations.

In summary, this for loop is a crucial part of the DFS algorithm. It explores different combinations of words by adding each word to the current combination (`temp`), making a recursive call to explore further possibilities, and then backtracking by removing the last word to explore other combinations. This process continues until the base case is reached, and the algorithm explores all valid combinations.



### Terminating dfs

If end<=0

1. `String path = temp.get(temp.size() - 1);`: It retrieves the last element from the `temp` ArrayList. This represents the last word in the current combination.

2. `for (int i = temp.size() - 2; i >= 0; i--)`: This loop iterates over the remaining elements in the `temp` ArrayList (excluding the last element). It starts from the second-to-last element and goes backward.

3. `path += " " + temp.get(i);`: It appends each word to the `path` variable, separated by a space. This creates a space-separated string of words representing the combination.

4. `result.add(path);`: It adds the combined string (`path`) to the `result` HashSet. The `result` HashSet is used to store unique combinations.

5. `return;`: It exits the current recursive call since the termination condition is met.











``` java

import java.util.*;

public class Solution {
    public static ArrayList<String> wordBreak(String A, ArrayList<String> B) {
        ArrayList<String>[] dp = new ArrayList[A.length() + 1];
        dp[0] = new ArrayList<>();

        for (int i = 0; i <= A.length(); i++) {
            if (dp[i] == null)
                continue;

            for (String word : B) {
                int length = word.length();
                int end = i + length;

                if (end > A.length())
                    continue;

                if (A.substring(i, end).equals(word)) {
                    if (dp[end] == null)
                        dp[end] = new ArrayList<>();
                    dp[end].add(word);
                }
            }
        }

        ArrayList<String> result = new ArrayList<>();
        HashSet<String> resultSet = new HashSet<>();
        if (dp[A.length()] == null)
            return result;

        ArrayList<String> temp = new ArrayList<>();
        dfs(dp, A.length(), resultSet, temp);

        result.addAll(resultSet);
        Collections.sort(result);
        return result;
    }

    public static void dfs(ArrayList<String>[] dp, int end, HashSet<String> result, ArrayList<String> temp) {
        if (end <= 0) {
            String path = temp.get(temp.size() - 1);
            for (int i = temp.size() - 2; i >= 0; i--)
                path += " " + temp.get(i);
            result.add(path);
            return;
        }

        for (String str : dp[end]) {
            temp.add(str);
            dfs(dp, end - str.length(), result, temp);
            temp.remove(temp.size() - 1);
        }
    }

    public static void main(String[] args) {
        ArrayList<String> B = new ArrayList<>();
        B.add("cat");
        B.add("cats");
        B.add("and");
        B.add("sand");
        B.add("dog");

        String A = "catsanddog";
        System.out.println("List is : " + B);
        System.out.println("String is  : " + A);
        ArrayList<String> result = wordBreak(A, B);

        for (String s : result)
            System.out.println(s);
    }
}

```


Testcase 
Input 1:
    A = "catsanddog",
    B = ["cat", "cats", "and", "sand", "dog"]

Output 1:
    ["cat sand dog", "cats and dog"]

Explanation: 

```
List is : [cat, cats, and, sand, dog]
String is  : catsanddog
Checking dp[0]: []
  i: 0, word: cat, end: 3
    dp[3] is null, creating a new ArrayList.
    Adding word 'cat' to dp[3]
    Updated dp[3]: [cat]
  i: 0, word: cats, end: 4
    dp[4] is null, creating a new ArrayList.
    Adding word 'cats' to dp[4]
    Updated dp[4]: [cats]
  i: 0, word: and, end: 3
  i: 0, word: sand, end: 4
  i: 0, word: dog, end: 3

Dynamic Programming Array (after iteration 0):
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: null
  dp[8]: null
  dp[9]: null
  dp[10]: null
dp[1] is null, skipping to the next iteration.
dp[2] is null, skipping to the next iteration.
Checking dp[3]: [cat]
  i: 3, word: cat, end: 6
  i: 3, word: cats, end: 7
  i: 3, word: and, end: 6
  i: 3, word: sand, end: 7
    dp[7] is null, creating a new ArrayList.
    Adding word 'sand' to dp[7]
    Updated dp[7]: [sand]
  i: 3, word: dog, end: 6

Dynamic Programming Array (after iteration 3):
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: [sand]
  dp[8]: null
  dp[9]: null
  dp[10]: null
Checking dp[4]: [cats]
  i: 4, word: cat, end: 7
  i: 4, word: cats, end: 8
  i: 4, word: and, end: 7
    Adding word 'and' to dp[7]
    Updated dp[7]: [sand, and]
  i: 4, word: sand, end: 8
  i: 4, word: dog, end: 7

Dynamic Programming Array (after iteration 4):
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: [sand, and]
  dp[8]: null
  dp[9]: null
  dp[10]: null
dp[5] is null, skipping to the next iteration.
dp[6] is null, skipping to the next iteration.
Checking dp[7]: [sand, and]
  i: 7, word: cat, end: 10
  i: 7, word: cats, end: 11
    Skipping, end is greater than A length.
  i: 7, word: and, end: 10
  i: 7, word: sand, end: 11
    Skipping, end is greater than A length.
  i: 7, word: dog, end: 10
    dp[10] is null, creating a new ArrayList.
    Adding word 'dog' to dp[10]
    Updated dp[10]: [dog]

Dynamic Programming Array (after iteration 7):
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: [sand, and]
  dp[8]: null
  dp[9]: null
  dp[10]: [dog]
dp[8] is null, skipping to the next iteration.
dp[9] is null, skipping to the next iteration.
Checking dp[10]: [dog]
  i: 10, word: cat, end: 13
    Skipping, end is greater than A length.
  i: 10, word: cats, end: 14
    Skipping, end is greater than A length.
  i: 10, word: and, end: 13
    Skipping, end is greater than A length.
  i: 10, word: sand, end: 14
    Skipping, end is greater than A length.
  i: 10, word: dog, end: 13
    Skipping, end is greater than A length.

Dynamic Programming Array (after iteration 10):
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: [sand, and]
  dp[8]: null
  dp[9]: null
  dp[10]: [dog]

Final Dynamic Programming Array:
  dp[0]: []
  dp[1]: null
  dp[2]: null
  dp[3]: [cat]
  dp[4]: [cats]
  dp[5]: null
  dp[6]: null
  dp[7]: [sand, and]
  dp[8]: null
  dp[9]: null
  dp[10]: [dog]
Adding word "dog" to temp: [dog]
Adding word "sand" to temp: [dog, sand]
Adding word "cat" to temp: [dog, sand, cat]
Adding combination to result: "cat sand dog"
Backtracking. Temp after removal: [dog, sand]
Backtracking. Temp after removal: [dog]
Adding word "and" to temp: [dog, and]
Adding word "cats" to temp: [dog, and, cats]
Adding combination to result: "cats and dog"
Backtracking. Temp after removal: [dog, and]
Backtracking. Temp after removal: [dog]
Backtracking. Temp after removal: []

Final Result:
cat sand dog
cats and dog
```


Code to print above


``` java

     import java.util.*;

public class Main {
    public static ArrayList<String> wordBreak(String A, ArrayList<String> B) {
        ArrayList<String>[] dp = new ArrayList[A.length() + 1];
        dp[0] = new ArrayList<>();

        for (int i = 0; i <= A.length(); i++) {
            if (dp[i] == null) {
                System.out.println("dp[" + i + "] is null, skipping to the next iteration.");
                continue;
            }

            System.out.println("Checking dp[" + i + "]: " + dp[i]);

            for (String word : B) {
                int length = word.length();
                int end = i + length;

                System.out.println("  i: " + i + ", word: " + word + ", end: " + end);

                if (end > A.length()) {
                    System.out.println("    Skipping, end is greater than A length.");
                    continue;
                }

                if (A.substring(i, end).equals(word)) {
                    if (dp[end] == null) {
                        System.out.println("    dp[" + end + "] is null, creating a new ArrayList.");
                        dp[end] = new ArrayList<>();
                    }

                    System.out.println("    Adding word '" + word + "' to dp[" + end + "]");
                    dp[end].add(word);

                    // Print the updated dp array
                    System.out.println("    Updated dp[" + end + "]: " + dp[end]);
                }
            }

            // Print dp array after each iteration inside the loop
            System.out.println("\nDynamic Programming Array (after iteration " + i + "):");
            for (int j = 0; j < dp.length; j++) {
                System.out.println("  dp[" + j + "]: " + dp[j]);
            }
        }

        ArrayList<String> result = new ArrayList<>();
        HashSet<String> resultSet = new HashSet<>();
        if (dp[A.length()] == null)
            return result;

        System.out.println("\nFinal Dynamic Programming Array:");
        for (int i = 0; i < dp.length; i++) {
            System.out.println("  dp[" + i + "]: " + dp[i]);
        }

        ArrayList<String> temp = new ArrayList<>();
        dfs(dp, A.length(), resultSet, temp);

        result.addAll(resultSet);
        Collections.sort(result);
        return result;
    }

    // ... (rest of the code remains unchanged)


    public static void dfs(ArrayList<String>[] dp, int end, HashSet<String> result, ArrayList<String> temp) {
    if (end <= 0) {
        String path = temp.get(temp.size() - 1);
        for (int i = temp.size() - 2; i >= 0; i--)
            path += " " + temp.get(i);
        result.add(path);
        System.out.println("Adding combination to result: \"" + path + "\"");
        return;
    }

    for (String str : dp[end]) {
        temp.add(str);
        System.out.println("Adding word \"" + str + "\" to temp: " + temp);
        dfs(dp, end - str.length(), result, temp);
        temp.remove(temp.size() - 1);
        System.out.println("Backtracking. Temp after removal: " + temp);
    }
}


    public static void main(String[] args) {
        ArrayList<String> B = new ArrayList<>();
        B.add("cat");
        B.add("cats");
        B.add("and");
        B.add("sand");
        B.add("dog");

        String A = "catsanddog";
        System.out.println("List is : " + B);
        System.out.println("String is  : " + A);
        ArrayList<String> result = wordBreak(A, B);

        System.out.println("\nFinal Result:");
        for (String s : result)
            System.out.println(s);
    }
}

```
