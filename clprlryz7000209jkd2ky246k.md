---
title: "[Leetcode]46. Permutations"
datePublished: Tue Dec 05 2023 00:29:28 GMT+0000 (Coordinated Universal Time)
cuid: clprlryz7000209jkd2ky246k
slug: leetcode46-permutations

---

![image](https://github.com/LB-Brandon/algorithm/assets/84883277/65759e3b-dc5e-46e1-8387-08d4d32cc13f)

# Key Idea
- nums를 기준으로 순회한다
- 순회하며 요소를 path 담고 해당 요소를 뺀 나머지 nums를 가지고 newNums를 초기화한 것을 DFS의 매개변수로 호출에 다시 nums를 기준으로 순회한다
- nums 의 길이가 0 이 되면 탈출조건이 만족된다
- return 후에는 이전 작업을 pop() 으로 복구시킨다

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