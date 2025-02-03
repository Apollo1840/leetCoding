# Islands

Islands number/perimeter/area.
(200/463/695)

Advanced: inlands bridge(934)

#### 200 Islands number
Task: count number of islands in a 0-1 matrix.


Solution: Outter loop enumerate 0-1 matrix to find "1"s to start the elimination queue. 
the bfs do the elimination job.

The elimination happens when find "1", alter it and then put it into queue. 

```python

def bfs(self, q, grid, m, n):     
    # shrink the island
              
    while q:
        lq = len(q)
        for _ in range(lq):
            i, j = q.popleft()
            for di in self.directions:
                i_ = i + di[0]
                j_ = j + di[1]
                if self.withinGrid(i_, j_,m,n) and grid[i_][j_] == "1":
                    grid[i_][j_] = "0"
                    q.append((i_, j_))

```

#### 463 Island perimeter
Task: calculate the perimeter of an island in a 0-1 matrix

Solution: count when explore alters or on the boundary. Note to mark the visited as "-1".

```python

def bfs(self, count, q, grid, m, n):     
    # explore the island         
    while q:
        lq = len(q)
        for _ in range(lq):
            i, j = q.popleft()
            for di in self.directions:
                i_ = i + di[0]
                j_ = j + di[1]
                if not self.withinGrid(i_, j_, m, n) or grid[i_][j_] == 0:
                    count += 1
                elif grid[i_][j_] == 1:
                    grid[i_][j_] = -1
                    q.append((i_, j_))
    return count


```

#### 695 Island area
Task: find max area island in a 0-1 matrix

Solution: count when find "1".

```python

def bfs(self, area, q, grid, m, n):     
    # shrink the island
              
    while q:
        lq = len(q)
        for _ in range(lq):
            i, j = q.popleft()
            for di in self.directions:
                i_ = i + di[0]
                j_ = j + di[1]
                if self.withinGrid(i_, j_, m, n) and grid[i_][j_] == 1:
                    area += 1
                    grid[i_][j_] = 0
                    q.append((i_, j_))
                    
    return area

```

#### 1020 Island area II
Task: count the area of the TRUE islands. 


#### 1162 Farest from land
Task: return fast distance from sea to land.

#### 934 Island bridge
Task: find shortest bridge between two islands on a 0-1 matrix map.

```python

def shortestBridge(self, grid):
    m, n = len(grid), len(grid[0])
    
    # use dfs to locate the first island, collect all indices pairs into self.visited
    self.visited = set()
    start_i, start_j = next((i, j) for i in range(m) for j in range(n) if grid[i][j])
    self.visited.add((start_i, start_j))
    self.dfs(grid, m, n, start_i, start_j)
    
    # use bfs to find shortest bridge
    queue = deque(self.visited)
    ans = self.bfs(queue, grid, m, n, 0)
    return ans

def dfs(self, grid, m, n, i, j):
    self.visited.add((i, j))
    for d in self.directions:
        i_ = i + d[0]
        j_ = j + d[1]
        if self.withinGrid(m, n, i_, j_) and (i_, j_) not in self.visited and grid[i_][j_]:
            self.dfs(grid, m, n, i_, j_)

def bfs(self, q, grid, m, n, ans):
    while q:
        lq = len(q)
        for _ in range(lq):
            i, j = q.popleft()
            for d in self.directions:
                i_ = i + d[0]
                j_ = j + d[1]
                if self.withinGrid(m,n, i_, j_) and (i_, j_) not in self.visited:
                    if grid[i_][j_]:
                        return ans

                    self.visited.add((i_, j_))
                    q.append((i_, j_))
        ans += 1

```

#### 1926 shiping out
Task: shorest time to escape the grid 