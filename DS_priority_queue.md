# PriorityQueue

PQ is often used to sort/query extrems.

Normally, we put a tuple of information into the heap, 
and take them out with order of the first entry and following entry is the information we care.

typical case:
- string: (-freq, char)
- array: (value, index)

## All-in & All-out

- 1337 k-weakest rows
- 506 Rank & Medal
- 451 Sortby freq

## All-in & All-out with replacement

Those challenges ofter ends with empty of the heap:

```python

h = ...
heapify(h)
while h:
    (...)

```

### Take Two
Take two (first and second) item out of the heap.

- 1046 Stone smash
- 2335 Fill cups


```python
h = ...
heapify(h)
while h:
    a = -heappop(h)
    if not h:
        ...
    else:
        a2 = -heappop(h)
```

Examples:

```python

# Smash the stones until last
def lastStoneWeight(self, stones: List[int]) -> int:
    h = [-s for s in stones]
    heapify(h)  # pass all negated values into the min-heap
    while h:
        biggest_stone = -heappop(h)  # the heaviest stone
        if not h:  # s1 is the remaining stone
            return biggest_stone
        else:
            biggest_stone2 = -heappop(h)  # the second-heaviest stone; s2 <= s1
            if biggest_stone == biggest_stone2:
                pass
            else:
                heappush(h, -(biggest_stone-biggest_stone2))  # push the NEGATED value of s1-s2; i.e., s2-s1
    return 0  # if no more stones remain

# Fill 3 cups to given requirements, either fill one cup or fill two cups with 1
def fillCups(self, amount: List[int]) -> int:
    h = [-a for a in amount if a!=0]
    heapify(h)
    c = 0
    while h:
        highest_requirement = -heappop(h)
        if not h:
            if highest_requirement-1>0:
                heappush(h, -(highest_requirement-1))
        else:
            highest_requirement2 = -heappop(h)
            if highest_requirement-1>0:
                heappush(h, -(highest_requirement-1))
            if highest_requirement2-1>0:
                heappush(h, -(highest_requirement2-1))     
        c += 1
    return c
                

```

### Others
Often with greedy solution:

- 767 Distinctive adjacent

## Gradually-in & Gradually-out

Those are typically the hardest application of PQ.

### Array-based: Sections
- 253 Meeting Rooms II
- 1094 Car Pooling

def. **Sections**: list of `(start, end)`.

Often, the heap stores the `(end, *)`.

**Meeting rooms II** (253)

Task: Given sections, figure out how many meeting rooms required.

Solution: use PQ to track the first available room and how many rooms are been used.
The heap stores section `to`s.

```python

def minMeetingRooms(self, intervals):
        if len(intervals) <= 1:
            return len(intervals)
        
        intervals.sort(key=lambda x: (x[0], x[1]))  # 按照开始时间排序，若相同则按结束时间排序
        
        h = [intervals[0][1]]
        heapify(h)  # 仅存储会议结束时间
        
        result = 1  # len(h)
        for i in range(1, len(intervals)):
            start, end = intervals[i]
            
            # here you can also empty all unused meeting rooms, but it is not necessary.
            if start >= h[0]:  # 当前会议开始时间 >= 最早结束的会议时间
                heapq.heappop(h)  # 复用会议室
            
            heapq.heappush(h, end)  # 添加新的会议结束时间
            result = max(result, len(h))  # 计算最多需要多少个会议室
        
        return result

```

**Car pooling** (1094)

Task: Given sections with num_passengers, judge whether it possible to taking them.

```python

def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
    if len(trips) == 1:
        return trips[0][0] <= capacity
    
    trips.sort(key=lambda x: x[1])
    
    h = [(trips[0][2], trips[0][0])]
    heapify(h)
    
    took = trips[0][0]
    if took > capacity:
        return False
    
    for i in range(1, len(trips)):
        n_passenger, loc_start, loc_end = trips[i]
        
        # empty the car if it already reached target location.
        while h and loc_start >= h[0][0]:  
            _, p = heappop(h)
            took -= p
            
        heappush(h, (loc_end, n_passenger))
        
        # load the new passengers and check capacity.
        took += n_passenger
        if took > capacity:
            return False
            
    return True
                


```

### Tree-based
- 743 Network delay
- 787 Cheapest Path
- (Grid) 778 Flooding swim

**Network delay**

Solution: `(arrive_time, node)` in heap.

```python

min_heap = [(0, source)]   # (arrive_time, node)
visited = {}
while min_heap:
    time, node = heapq.heappop(min_heap)
    if node not in visited:
        visited[node] = time
        for node_i, time_i in adjMap[node]:
            heapq.heappush(min_heap, (time + time_i, node_i))
```


**Cheapest path**

Solution: `(cost, node, stops)` in heap.

### Others
- (simple) 502 IPO
- 378 K-th smallest in matrix 
- 218 Skyline