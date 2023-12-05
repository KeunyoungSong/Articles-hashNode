---
title: "[Leetcode]17. Letter Combinations of a Phone Number"
datePublished: Tue Dec 05 2023 02:20:58 GMT+0000 (Coordinated Universal Time)
cuid: clprprcz9000f08ic3vbkgl3p
slug: leetcode17-letter-combinations-of-a-phone-number-1

---

![image](https://github.com/LB-Brandon/algorithm/assets/84883277/687a97ad-bde8-44eb-9e3e-17066773d9bf)

# Key Idea
- Pass `start` and `path` to DFS().
    - `start` denotes the index to start exploring from, initialized to 1.
    -  It increments with each recursion to avoid reusing elements.
   - `path` keeps track of the current permutation being formed.
- The escape condition triggers when the length of `path` equals the desired combination length `k`.
- After each recursion, use `pop()` to backtrack and explore other combinations.

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def dfs(start, path):
            # Escape condition
            if len(path) == k:
                result.append(path.copy())
                return
            for value in range(start, n+1):
                path.append(value)
                dfs(value+1, path)  # Move to the next index in recursion
                path.pop()  # Backtrack
        
        result = []
        dfs(1, [])  # Start DFS from the beginning
        return result
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0077-combinations]