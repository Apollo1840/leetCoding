# Count
Return of the algorithm is a **Number**.

Typical clues:
- Adds-up (`count +=1`)
- Caching (min/max, `res = min(res, ...)`)
- Binary search (`if isSuccess(mid):`)
- Accumulative (using recursive logic) 

## Adds-up

- adds-up when meet condition:
  - (200) Islands number
  - (547, 1319) Province number
  - (463) Islands perimeter
  - (695) Islands area
  - (771) Jewels & stones
- adds-up when PQ pop:
  - (2335) Fill cups
  - (621) Task Scheduler
- adds-up when BFS expand:
  - (934) Islands bridge
  - (1162) Farest from land
  - (1926) Escape Maze
  - (1284) Minimum Flips: brutal force.
  - (2385) Infection time: until BFS stop expanding.

## Caching
Possible results are enumerated, we use 
- `res = max(res, ...)` or
- `res = min(res, ...)`
to record the result.

### Greedy
If only a subset of the possible results is enumerated.
i.e. we find out ways to exclude some result.


- (121) Best Buysale
- (53) Max sub-array
- (152) Max-prod sub-array (hint: without considering zero first)
- (11) Largest container
- (128) Longest consecutive

### Others
Or naturally update the result during the process.
- (42) Trapping rain
- (253) Meet rooms II
- (778) Flooding swim
- (543) Diameter of BT

## Binary search
see `BS Computing` in `../2pointers_BS.md`.
- (69) Sqrt
- (410) Minimum Largest sum
- (875) KoKo
- (1011) D-day shipping
- (1840) Maxmum height ?

Template:

```python

# HalfOpen to find minimum achievable solution
def MinSolver(self, ...):
    l, r = ...
    while l < r:
        mid = (l +r)//2
        if self.achieve(*):
            # print("ok with {}".format(mid))
            r = mid
        else:
            l = mid + 1
    return l
```


## Accumulative counting
With recursive logic.
- Recursive(DC/DFS)
  - (50) Pow(x, n)
- DP  
  - (509) Fibonacci
  - (62, 63, 64) Unique Path
  - (70) Climbing stairs
  - (746) Climbing stairs II
  - (45) Jump game II
  - (198) House Rabber
  - (322) Coin Change
  - (300) longest sub-increasing

## Others
### processed counting
with PQ to process the result step by step
- (1046) Stone smash
- (787) Cheapest path
- (743) Network Delay
- (502) IPO


### subtractive
- (2055) Plates & candles
- (560) k-sum sub-array

### calculation
- (1128) Domino pairs
- (447) Boomerang amount