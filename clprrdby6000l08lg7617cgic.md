---
title: "[Leetcode]39. Combination Sum"
datePublished: Tue Dec 05 2023 03:06:02 GMT+0000 (Coordinated Universal Time)
cuid: clprrdby6000l08lg7617cgic
slug: leetcode39-combination-sum

---

# Key Idea
- Similar to '[Leetcode]46. Permutations' but allows the same start.
    - [Leetcode]46. Permutations: 
       https://hashnode.com/edit/clprlryz7000209jkd2ky246k
- Check if the sum of the previous path is equal to the target.

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(start, path):
            if sum(path) == target:
                result.append(path.copy())
            if sum(path) > target:
                return
            
            for i in range(start, len(candidates)):
                path.append(candidates[i])
                dfs(i, path)
                path.pop()
        
        result = []
        dfs(0, [])
        return result
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0039-combination-sum]