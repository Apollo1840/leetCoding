# DFS Backtracking

## 17 Phone Typing
classic backtracking

Expand version:
```python

def letterCombinations(self, digits: str) -> List[str]:
    if not digits:
            return []

    self.res = list()
    self.dfs(digits, list())  # list() are starting path
    return self.res

def dfs(self, digits, path):
    if not digits:
        self.res.append("".join(path))
        return
    
    for letter in self.phone_map[digits[0]]:
        path = path + [letter]
        self.dfs(digits[1:], path)
        path.pop()  # The BACK-tracking step


```


Shorter version of `dfs()`:
```python

def dfs(self, digits, path):
    if not digits:
        self.res.append("".join(path))
        return
    
    for letter in self.phone_map[digits[0]]:
        self.dfs(digits[1:], path + [letter])

```


## 797 All Source2Target

Task: return all paths from source(0) to target(n) given a graph as adjancency list.

```python

def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
    self.res = list()
    self.target = len(graph)-1
    self.dfs(graph, [0])
    return self.res

def dfs(self, graph, path):
    # finish ends
    if path[-1]==self.target:
        self.res.append(path[:])
        return

    # recursive
    for j in graph[path[-1]]:
        path += [j]
        self.dfs(graph, path)
        path.pop() # The BACK-tracking step
```


## 131 Palindrome partition
Task: return all partition methods on a string to make each substring palindrome.
```python

def backtrack(start, path):
    if start == n:
        result.append(path[:])
        return
    
    for end in range(start, n):
        if dp[start][end]:  # Check if s[start:end+1] is a palindrome
            path += [s[start:(end + 1)]]
            backtrack(end + 1, path)
            path.pop()

backtrack(0, [])

```