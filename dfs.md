# DFS


DFS to collect:

```python

visited = set()
dfs(visited, point)
return visited

def dfs(visited, point):
    if isGood(point):
        visited.add(point)
        for i in allRelated(point):
            dfs(visited, i)
```

DFS to judge:

```python

dfs(inputs)

def dfs(inputs):
    # None finish
    if notValid(inputs):
        return False
    
    # success finish
    if isSuccess(inputs):
        return True
    
    # recursive
    inputs_ = update(inputs)
    return dfs(inputs_)


```

## Tree

### Path Sum (112)

Target: Judge whether (path sum == target) exists or not.

