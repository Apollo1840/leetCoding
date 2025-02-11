# DFS Backtracking
Often used in collection tasks, collect all valid solutions.
Also used in some judgement tasks where resource can not be reused.
## Methodology

```python

res = set()
dfs(resource, point)
return visited

def dfs(resource, point):
    if isValid(point):
        res.add(point)

    for i in allRelated(resource, point):
        dfs(reduced_resource, i)
        
        # traceback i & free resource 
        backtrack()
```

### On Array

#### 17 Phone Typing
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
    # finish ends
    if not digits:
        self.res.append("".join(path))
        return
    
    # recursive
    for letter in self.phone_map[digits[0]]:
        path = path + [letter]
        self.dfs(digits[1:], path)
        path.pop()  # The BACK-tracking step


```


Shorter version of `dfs()`:
```python

def dfs(self, digits, path):
    # finish ends
    if not digits:
        self.res.append("".join(path))
        return
        
    # recursive
    for letter in self.phone_map[digits[0]]:
        self.dfs(digits[1:], path + [letter])

```

#### 46 Permutations
Solution: Use `visited` dictionary to mark consumed characters.

```python

def dfs(self, nums, used, path):
    # None ends
    # no need

    # finish ends
    if len(path)==len(used):
        self.res.append(path[:])
        return 

    # recursive
    for i, n in enumerate(nums):
        if used[i]:
            continue

        # prepare backtracking
        used[i] = True
        path.append(n)

        self.dfs(nums, used, path)

        # here is core of the BACK-tracking
        path.pop()
        used[i] = False  



```
#### 78 Subsets 
Tasks: return all possible subsets

```python

def dfs(self, i, path):
    # finish ends
    if i == len(self.nums):
        self.res.append(path[:])
        return
    
    # include i
    path += [self.nums[i]]
    self.dfs(i+1, path)

    # not include i
    path.pop()
    self.dfs(i+1, path)
        
```


#### 131 Palindrome partition
Task: return all partition methods on a string to make each substring palindrome.

Solution: **DFS Backtracking for enumerate all valid partitions**
```python

def dfs(self, start, n, path):
    # finish ends
    if start == n:
        self.res.append(path[:])
        return
    
    # recursive
    for end in range(start, n):
        if dp[start][end]:  # Check if s[start:end+1] is a palindrome
            path += [s[start:(end + 1)]]
            self.dfs(end + 1, n, path)
            path.pop()

```


#### 126 Word Ladder II

Straight forward:
```python

def dfs(self, word, endWord, n, level, path):
    if word == endWord:
        self.res.append(path[:])
        return

    if level == n:
        return

    for item in nodes[level]:
        if connected(item, word):
            path += [item]
            self.dfs(item, endWord, n, level+1, path)
            path.pop()
```

Actually better: 
```python
# only consider the valid path, hence much faster            
def dfs_reverse(self, word, beginWord, level, path):
    if word == beginWord:
        self.res.append(path[::-1])
        return

    if level < 0:
        return

    for item in self.nodes[level]:
        if self.areNeighbors(item, word):
            path += [item]
            self.dfs(item, beginWord, level-1, path)
            path.pop()

```

### On Grid

#### 79 Word search

Solution: Use `#` to mark consumed characters.

```python

# check whether can find word, start at (i,j) position    
def dfs(self, board, i, j, word):

    # None ends & incorrect ends
    if not self.withinGrid(board, i, j):
        return False
    elif word[0]!=board[i][j]:
        return False

    # Finish ends
    if len(word) == 1 and word[0] == board[i][j]:
        return True

    # Backtracking prepare
    tmp = board[i][j]  # first character is found, check the remaining part
    board[i][j] = "#"  # avoid visit again

    # recursive
    for d in self.directions:
        res = self.dfs(board, i+d[0], j+d[1], word[1:])
        if res:
            break

    # Backtracking
    board[i][j] = tmp   # here is the core concept of BACK-tracking 

    return res



```

### On Graph

#### 797 All Source2Target

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


