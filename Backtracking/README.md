


## 🔄 Why do we add the copy of the current to the result?:

```java
result.add(new ArrayList<>(current));
```

in both **subsets** and **combination sum**?

---

## 📦 `result` is the box 📦

It's the **final list** where we store all valid combinations.

## 🧱 `current` is the builder 🧱

It’s the **temporary list** you're building as you explore choices.

---

## 🔥 Key Idea:

We don’t want to save a **reference** to `current`.
We want to save a **copy** of it **right now**.

Because later, we will **change** `current` (backtrack).
If we added `current` directly, all the values in `result` would **change too**.

---

### 🛠️ That's why we do:

```java
new ArrayList<>(current)
```

This creates a **new list** (a snapshot), and adds that to `result`.

---

## 📊 Visual Example:

Let’s say you're building:

```java
current = [1, 2]
```

### ✅ You found a valid subset or combo. So you do:

```java
result.add(new ArrayList<>(current));  // Save [1,2]
```

Then you keep exploring and backtrack:

```java
current.remove(current.size() - 1);  // Now current = [1]
```

Now:

* `result` still contains `[1, 2]`
* `current` is `[1]`

✅ Perfect! You’ve saved that path, and you’re ready to explore the next.
