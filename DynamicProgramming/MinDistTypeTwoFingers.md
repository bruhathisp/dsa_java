Here’s your content beautifully formatted in **Markdown**, ready to copy and paste anywhere (e.g., GitHub, Obsidian, Notion):

```markdown
📌 `dfs(0, -1, -1)` — Starting point (No fingers placed yet, current = `'C'`)

dfs(0, -1, -1) - 'C'
├── Finger 1 types 'C' → dfs(1, C, -1)
│   ├── Finger 1 types 'A' → dfs(2, A, -1)
│   │   └── Finger 1 types 'T' → dfs(3, T, -1) = 0
│   │       ↳ Cost: dist(A, T) = 4
│   │   ↳ Total = 2 (C→A) + 4 = 6
│   
├── Total cost1 = 6
│
├── Finger 2 types 'C' → dfs(1, -1, C)
│   ├── Finger 1 types 'A' → dfs(2, A, C)
│   │   └── Finger 1 types 'T' → dfs(3, T, C) = 0
│   │       ↳ Cost: dist(A, T) = 4
│   │   ↳ Total = 0 (start) + 4 = 4
│   ├── Finger 2 types 'A' → dfs(2, -1, A)
│   │   └── Finger 2 types 'T' → dfs(3, -1, T) = 0
│   │       ↳ Cost: dist(A, T) = 4
│   │   ↳ Total = 2 (C→A) + 4 = 6
├── Total cost2 = 4

```

✅ **Final Decision**

min(cost1=6, cost2=4) = 4

💬 **Visual Summary**

* The tree explores all possibilities of which finger types each character.
* `cost1` tries the first finger doing most of the work.
* `cost2` considers the second finger stepping in.
* At every level, we go deep into each branch before comparing.
* memo so that we don't get a TLE(Time limit exceeded)



``` java
import java.util.HashMap;
import java.util.Map;

class Solution {
    int[][] pos = new int[26][2];
    Map<String, Integer> memo = new HashMap<>();

    public int minimumDistance(String word) {
        // Initialize keyboard positions
        for (int i = 0; i < 26; i++) {
            pos[i][0] = i / 6;
            pos[i][1] = i % 6;
        }

        return dfs(word, 0, -1, -1);
    }

    int dfs(String word, int i, int f1, int f2) {
        if (i == word.length()) return 0;

        String key = i + "," + f1 + "," + f2;
        if (memo.containsKey(key)) return memo.get(key);

        int curr = word.charAt(i) - 'A';

        // Option 1: finger 1 types it
        int move1 = (f1 == -1) ? 0 : dist(f1, curr);
        int cost1 = move1 + dfs(word, i + 1, curr, f2);

        // Option 2: finger 2 types it
        int move2 = (f2 == -1) ? 0 : dist(f2, curr);
        int cost2 = move2 + dfs(word, i + 1, f1, curr);

        int result = Math.min(cost1, cost2);
        memo.put(key, result);
        return result;
    }

    int dist(int a, int b) {
        return Math.abs(pos[a][0] - pos[b][0]) + Math.abs(pos[a][1] - pos[b][1]);
    }
}
```
