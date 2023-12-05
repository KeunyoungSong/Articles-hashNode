---
title: "[Leetcode]17. Letter Combinations of a Phone Number"
datePublished: Mon Dec 04 2023 13:05:17 GMT+0000 (Coordinated Universal Time)
cuid: clpqxc3ze000509k01hpi5nco
slug: leetcode17-letter-combinations-of-a-phone-number

---

# Key Idea
## Iterative
- 3 for-loops are required for the iterative solution.
- It's not feasible to use a single for-loop for all lists.
    - Accumulate the characters mapped to `digits` in the `result` at each loop.

## Recursive
- 2 for-loops are required for the recursive solution.
- It's not feasible to use a single for-loop for all lists.
    - Set the index and path as arguments for DFS. Traverse based on the index, accumulating characters mapped to `digits` in the `path` at each DFS.

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        mapping = {
            '2' : "abc",
            '3' : "def",
            '4' : "ghi",
            '5' : "jkl",
            '6' : "mno",
            '7' : "pqrs",
            '8' : "tuv",
            '9' : "wxyz"
        }
        
        # iterative
        # if not digits:
        #     return []
        # result = ['']
        # for d in digits:
        #     q = result.copy()
        #     result = []
        #     for s in q:
        #         for c in mapping[d]:
        #             result.append(s+c)
        
        # backtracking        
        def dfs(start, path):
            if len(path) == len(digits):
                result.append(path)
                return
        
            for i in range(start, len(digits)):
                for c in mapping[digits[i]]:
                    dfs(i+1, path+c)
        
        if not digits:
            return []
        
        result = []
        dfs(0, '')   
        return result
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0017-letter-combinations-of-a-phone-number]