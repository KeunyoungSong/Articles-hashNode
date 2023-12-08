---
title: "[Leetcode]332. Reconstruct Itinerary"
datePublished: Thu Dec 07 2023 16:27:28 GMT+0000 (Coordinated Universal Time)
cuid: clpvevo3f000008jq1w7jhnq6
slug: leetcode332-reconstruct-itinerary

---

# Key Idea
### Solution1 : Backtracking
- Must traverse all possible destinations.
- There is a chance that we may select a node meeting the smallest lexical order visit condition, but with no return back. Such cases may not be resolved with a simple DFS as you can't pass through all possible destinations.
- Use Backtracking.
- Reverse the result since decisions are added from the last decision space.

### Solution2: Backtracking with stack [Optimization of Solution 1]
- Make the dictionary in reverse order.
- Now we can use `pop()` instead of `pop(0)`.

### Solution3: Only Stack
- Utilizing the characteristics of the stack data structure.

## Solution1 : Backtracking
```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = collections.defaultdict(list)
        
        # make sorted adjacency list in smallest lexical order
        for ticket in sorted(tickets):
            f, t = ticket
            graph[f].append(t)
        
        def dfs(current):
            # explore all possible destinations
            while graph[current]:
                dfs(graph[current].pop(0))
            # record the decision from the last
            result.append(current)
            
        result = []
        dfs("JFK")
        # appending from last decision, so reverse the list
        return result[::-1]
```

## Solution2: Backtracking with stack
```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = collections.defaultdict(list)
        
        # make sorted adjacency list in smallest lexical order
        for ticket in sorted(tickets, reverse=True):
            f, t = ticket
            graph[f].append(t)
        
        def dfs(current):
            # explore all possible destinations
            while graph[current]:
                dfs(graph[current].pop())
            # record the decision from the last
            result.append(current)
            
        result = []
        dfs("JFK")
        # appending from last decision, so reverse the list
        return result[::-1]
```

## Solution3: Only Stack
```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = collections.defaultdict(list)
        
        # make sorted adjacency list in smallest lexical order
        for ticket in sorted(tickets):
            f, t = ticket
            graph[f].append(t)
        
        stack, route = ["JFK"], []
        # explore all possible destinations
        while stack:
            while graph[stack[-1]]:
                stack.append(graph[stack[-1]].pop(0))
                # record the decision from the last
            route.append(stack.pop())
            
        # appending from last decision, so reverse the list
        return route[::-1]
```

%[https://github.com/LB-Brandon/algorithm/tree/main/0332-reconstruct-itinerary]