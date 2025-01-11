# DFS Backtracking

## 797 Source2Target

Task: return all paths from source(0) to target(n) given a graph as adjancency list.

```python

def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
    
    # initialize the group to store the results
    self.res = list()
    
    # start with initial point and result space_holder
    self.dfs(graph, len(graph), 0, list())
    
    return self.res

def dfs(self, graph, n, currNode, currPath):
    # finish ends
    if currNode==n-1:
        self.res.append(currPath + [currNode])
        return

    # recursive
    for j in graph[currNode]:
        currPath.append(currNode)
        self.dfs(graph, n, j, currPath)
        currPath.pop()  # The BACK-tracking step

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