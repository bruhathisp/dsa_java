**Longest String Chain** using the **"5-question format"** you used for matrix and DP problems 

---


To solve the problem using recursion and memoization, we define a function dfs(word) that returns the length of the longest chain ending at that word. If we’ve already computed the result for this word, we return the cached value from the memo. Otherwise, we try removing each character from the word to form a new, shorter word. For every such shortened word, if it exists in the input list, we recursively call dfs on it and compute the chain length. We keep track of the maximum length found from all such valid previous words, adding +1 to account for the current word. Finally, we store the result in the memo and return it.

## 🧠 Problem Recap

You're given a list of words.
Your goal:

> Find the **length of the longest string chain** you can form.
> A valid chain is when you can go from word A to B by **adding exactly one character anywhere** and B exists in the list.

---

## ❓ 1️⃣ Can I define a subproblem?

Yes!

We ask:

> “What is the **longest chain ending at** this word?”

Let’s define:

```java
dp[word] = length of longest chain ending at `word`
```

So if we compute `dp["bdca"]`, it could be:
`"a" → "ba" → "bda" → "bdca"` → length = 4

---

## 🔀 2️⃣ What decisions do I make at each word?

You try **removing one character** at every position in the word, like:

```
"bdca" → ["dca", "bca", "bda", "bdc"]
```

For each shortened word, if it's in the word list → it's a valid previous step in the chain.

So:

> "Can I build a longer chain by going one step **backward**?"

---

## 🧮 3️⃣ What do I track?

You track the **length of the longest chain ending at each word**.

We use a **memo table**:

```java
Map<String, Integer> memo
```

If we already computed `dfs("bda")`, we use memo instead of recalculating.

---

## 🧱 4️⃣ Base case?

There’s no fixed base case like a matrix border — instead:

> Every word has a default chain length of 1 (just itself).

So:

```java
if no valid smaller word: dp[word] = 1
```

---

## ✅ 5️⃣ Final result?

After processing all words:

> Return the **maximum value** of all `dp[word]` in the map.

Because the longest chain could end at any word.

```java
maxChain = max of all dfs(word)
```

``` java

import java.util.*;

public class Solution {
    Map<String, Integer> memo = new HashMap<>();
    Set<String> wordSet;

    public int longestStrChain(String[] words) {
        wordSet = new HashSet<>(Arrays.asList(words));
        int maxChain = 0;

        for (String word : words) {
            maxChain = Math.max(maxChain, dfs(word));
        }

        return maxChain;
    }

    private int dfs(String word) {
        if (memo.containsKey(word)) return memo.get(word);

        int maxLen = 1; // start from length 1 (itself)

        for (int i = 0; i < word.length(); i++) {
            String prev = word.substring(0, i) + word.substring(i + 1);
            if (wordSet.contains(prev)) {
                maxLen = Math.max(maxLen, 1 + dfs(prev));
            }
        }

        memo.put(word, maxLen);
        return maxLen;
    }
}

```
