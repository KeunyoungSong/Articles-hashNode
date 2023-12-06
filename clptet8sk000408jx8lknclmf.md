---
title: "[Leetcode]78. Subsets"
datePublished: Wed Dec 06 2023 06:50:02 GMT+0000 (Coordinated Universal Time)
cuid: clptet8sk000408jx8lknclmf
slug: leetcode78-subsets

---

# Key Idea
### Solution 1
- Divide branches into those that include each element as a subset and those that do not.

### Solution 2
- Utilize pass-by-value copy of path in DFS for calculations.

## Solution 1
```python
def subsets(self, nums: List[int]) -> List[List[int]]:
        subset = []
        def dfs(i):
            if i >= len(nums):
                result.append(subset.copy())
                return
            
            # decision to include nums[i]
            subset.append(nums[i])
            dfs(i + 1)
            
            #decision Not to include nums[i]
            subset.pop()
            dfs(i+1)
        
        result = []
        dfs(0)
        return result
```
## Solution 2
```python
def dfs(start, path):
            result.append(path)

            for i in range(start, len(nums)):
                dfs(i+1, path+[nums[i]])

    result = []
    dfs(0, [])
    return result
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0078-subsets]