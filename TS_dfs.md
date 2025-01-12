# DFS

## Ideaology

- In most cases, DFS can solve problems that are also solvable with BFS.
- DFS is often more intuitive when implemented recursively.
- DFS can be extended to backtracking methods, making it suitable for solving a wider range of collection problems.

## Methodology
DFS to collect:

```python

visited = set()
dfs(visited, point)
return visited

def dfs(visited, point):
    visited.add(point)
    for i in allRelated(point):
        if isGood(i) and i not in visited:
            dfs(visited, i)
```

eg. 547 Province Number.

DFS to judge:

```python
# pseudo code:

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
    for i in inputs_:
        info.add(dfs(i))
        
    return judge(info)


```

eg. 112 Path Sum

Sometimes, `dfs()` needs to return extra information for the inference of current node:

I call it **"DFS-II"**.

```python
# pseudo code:

dfs(inputs)

def dfs(inputs):
    # None finish
    if notValid(inputs):
        return False, extraInfo
    
    # success finish
    if isSuccess(inputs):
        return True, extraInfo
    
    # recursive
    inputs_ = update(inputs)
    for i in inputs_:
        info.add(dfs(i))
        
    return judge(info)


```


## Classic

### Tree

#### Path Sum (112)

Task: Judge whether (path sum == target) exists or not.

#### Diameter of Tree (543)
Task: longest length of leaf node to leaf node.

Solution: DFS-II

```python

def dfs(self, node):
    # None ends
    if node is None:
        return 0, 0
    
    # Recursive
    left_diameter, left_depth = self.dfs(node.left)
    right_diameter, right_depth = self.dfs(node.right)
    
    # update result of current node by children node
    diameter = max(left_diameter, right_diameter, left_depth + right_depth)
    
    # extra information
    curr_depth =  1 + max(left_depth, right_depth)

    return diameter, curr_depth
```

#### isBalance Tree (110)
Task: judge whether tree is balanced.

Solution: DFS-II

```python

def dfs(self, node):
    # None ends
    if node is None:
        return [True, 0]
    
    # Recursive
    left_balanced, left_height = self.dfs(node.left)
    right_balanced, right_height = self.dfs(node.right)

    # update result of current node by children node
    balanced = left_balanced and right_balanced and abs(left_height - right_height) <= 1
    
    # extra information
    curr_height = 1 + max(left_height, right_height)
    
    return balanced, curr_height
```

#### Valid BST (98)
Task: judge whether tree is binary search tree.

Solution: DFS-II

(slight more complex but same logic)
```python

def dfs(self, node):
    if node is None:
        return True, None, None
    
    # Recursive
    left_true, left_maximum, left_minimum = self.dfs(node.left)
    right_true, right_maximum, right_minimum = self.dfs(node.right)
    
    # update result of current node by children node
    left_condition = left_maximum < node.val if left_maximum is not None else True
    right_condition = right_minimum > node.val if right_minimum is not None else True
    curr_balance = left_true and right_true and left_condition and right_condition
    
    # extra information
    curr_minimum = node.val if left_minimum is None else left_minimum
    curr_maximum = node.val if right_maximum is None else right_maximum

    return curr_balance, curr_maximum, curr_minimum

```