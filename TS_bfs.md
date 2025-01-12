# BFS

We use queue to implement BFS, my convention is:
- hard in  (check conditions before add into queue) 
- easy out (dealt with that node before add into queue)

Challenges:
- (simple)(Grid) 733 Flood Fill

## Classic
### (Tree-based) 
- 993 Are cousins



### (Grid-based) Islands

Islands number/perimeter/area.
(200/463/695)

Advanced: inlands bridge(934)

#### Islands number
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

#### Island perimeter
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

#### Island area
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

#### Island bridge
Task: find shortest bridge between two islands on a 0-1 matrix map.

```python

    def shortestBridge(self, grid):
        m, n = len(grid), len(grid[0])
        
        # use dfs to locate the first island, collect all indices pairs into self.visited
        self.visited = set()
        start_i, start_j = next((i, j) for i in range(m) for j in range(n) if grid[i][j])
        self.dfs(grid, m, n, start_i, start_j)
        
        # use bfs to find shortest bridge
        queue = deque(self.visited)
        ans = self.bfs(queue, grid, m, n, 0)
        return ans
    
    # mark the first island as visited
    def dfs(self, grid, m, n, i, j):
        if (i, j) not in self.visited and grid[i][j]:
            self.visited.add((i, j))
            for d in self.directions:
                i_ = i + d[0]
                j_ = j + d[1]
                if self.withinGrid(m, n, i_, j_):
                    self.dfs(grid, m, n, i_, j_)
    
    # count length of the bridge
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

### (Graph-based) Course schedule

#### (1971) Exist Source2Target
Standard BFS with visited hashmap.

```python

q = deque([source])
visited = set()

while q:
    lq = len(q)
    for _ in range(lq):
        node = q.popleft()
        for t in adjmap[node]:
            if t == target:
                return True
            if t not in visited:
                visited.add(t)
                q.append(t)
return False


```


#### (207) Course schedule I
Task: Give prerequisite edge list, judge whether it is possible to finish all.

Solution: use bfs to collect free(in_degree becomes zero) nodes.

```python

def bfs(self, q, supp_adjmap, in_degree, finished):
    while q:
        lq = len(q)
        for _ in range(lq):
            take = q.popleft()
            finished.add(take)
            for course in supp_adjmap[take]:
                in_degree[course] -= 1
                if in_degree[course]==0:  # be "freed"
                    q.append(course)

```


#### (210) Course schedule II
Task: Give prerequisite edge list, return possible sequence of courses

Solution: same as before, instead of using finished as `set`, use `list`.
