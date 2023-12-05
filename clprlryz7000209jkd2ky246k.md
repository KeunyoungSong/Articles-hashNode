---
title: "[Leetcode]46. Permutations"
datePublished: Tue Dec 05 2023 00:29:28 GMT+0000 (Coordinated Universal Time)
cuid: clprlryz7000209jkd2ky246k
slug: leetcode46-permutations

---

![image](https://github.com/LB-Brandon/algorithm/assets/84883277/65759e3b-dc5e-46e1-8387-08d4d32cc13f)

# Key Idea
- Iterate through `nums`
- While traversing, store the element in the `path`. Then, initialize `newNums` with the remaining `nums` after excluding the current element, and recursively call DFS with `newNums` as the new set of numbers to consider.
- The escape condition is when the length of `nums` becomes 0.
- After returning from the recursion, restore the previous state of the operation using `pop()`.

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, path):
            # escape
            if len(nums) == 0:
                result.append(path.copy())
                # print('path', path)
                return
        
            for i in range(len(nums)):
                path.append(nums[i])
                newNums = [value for index, value in enumerate(nums) if index != i]
                dfs(newNums, path)
                path.pop()
            

        result = []
        dfs(nums, [])
        return result
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0046-permutations]