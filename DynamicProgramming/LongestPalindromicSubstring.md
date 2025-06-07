---

## 💡 PROBLEM: Longest Palindromic Substring

Given a string `s`, return the **longest substring** that reads the **same forward and backward**.

---

## 🔍 What Are We Trying to Do?

Find the **longest part** of the string that is a **palindrome**.

---

## ❓ Why "every single thing"?

### 🔸 1. We don’t know **where** the longest palindrome is.

It can start **anywhere** and end **anywhere**.

Examples:

* `"racecar"` → full string is a palindrome.
* `"abcxyzcba"` → longest palindrome might be `"xyx"` in the middle.

We can't **guess**. We must check **all substrings** to be sure.

---

### 🔸 2. Bigger palindromes depend on smaller palindromes.

Let’s say you’re checking if `"abccba"` is a palindrome:

* Check first and last character: `'a' == 'a'` ✅
* But you still have to know if `"bccb"` is a palindrome.

**This is where dynamic programming helps:**
We **already solved** `"bccb"` earlier. We just look it up.

---

## ⚙️ DP Table Explanation (2D Table)

We build a table `dp[i][j]`, where:

* `i` is the **start** index
* `j` is the **end** index
* `dp[i][j] == true` if `s[i..j]` is a palindrome

**Why?**

* So we can reuse results and not recompute the same thing again and again.

---

## 🛠️ The Rules Behind the Code

```java
if (s.charAt(i) == s.charAt(j)) {
  if (len == 1 || len == 2) {
    dp[i][j] = true;  // Single char or two equal chars like "aa"
  } else {
    dp[i][j] = dp[i + 1][j - 1];  // Look inside
  }
} else {
  dp[i][j] = false;
}
```

---

### 📊 Table Build-Up

We loop:

```java
for (int len = 1; len <= n; len++) {
    for (int i = 0; i <= n - len; i++) {
        int j = i + len - 1;
```

### 👇 What’s this doing?

* `len`: We are checking **substrings of increasing length**
* `i`: start index
* `j`: end index = `i + len - 1`

### 🧠 Why `i <= n - len`?

To **avoid going out of bounds**. You can only start a substring if it fits in the string.

---

## ✨ Internalize It Like This

### Step-by-step

| Length | What We’re Checking | Logic                                  |
| ------ | ------------------- | -------------------------------------- |
| 1      | Each char by itself | Always palindrome                      |
| 2      | Pairs of chars      | `'a' == 'a'`? → Yes: it's a palindrome |
| ≥3     | `s[i] == s[j]`?     | Yes → check `dp[i+1][j-1]`             |
|        |                     | No → `dp[i][j] = false`                |

---

## 🧠 Simple Analogy

> “Big palindromes are just small palindromes with matching ends.”

Dynamic programming lets you **build up** to big ones from the small ones **only once**.

---

## ✅ Final Code (Cleaned Version)

```java
public String longestPalindrome(String s) {
    int n = s.length();
    boolean[][] dp = new boolean[n][n];
    int start = 0, maxLen = 0;

    for (int len = 1; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j)) {
                if (len == 1 || len == 2) {
                    dp[i][j] = true;
                } else {
                    dp[i][j] = dp[i + 1][j - 1];
                }
            } else {
                dp[i][j] = false;
            }

            if (dp[i][j] && len > maxLen) {
                maxLen = len;
                start = i;
            }
        }
    }
    return s.substring(start, start + maxLen);
}
```

---

