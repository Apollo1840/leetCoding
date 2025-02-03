# Count
Return of the algorithm is a **Number**.

Typical clues:
- Adds-up (`count +=1`)
- Caching (min/max, `res = min(res, ...)`)
- Binary search (`if isSuccess(mid):`)
- Accumulative (using recursive logic) 

## Adds-up

- conditional counting
  - (200) Islands number
  - (547, 1319) Province number
  - (463) Islands perimeter
  - (695) Islands area
  - (771) Jewels & stones
- adds-up when PQ pop
  - (2335) Fill cups
  - (621) Task Scheduler
- adds-up when BFS expanding:
  - (934) Islands bridge
  - (1162) Farest from land
  - (1926) Escape Maze
  - (1284) Minimum Flips: brutal force.
  - (2385) Infection time: until BFS stop expanding.

## Caching
- (778) Flooding swim
- (253) Meet rooms II
- (11) Largest container
- (42) Trapping rain
- (121) Best Buysale
- (560) k-sum sub-array
- (128) Longest consecutive

## Binary search
- (69) Sqrt
- (410) Minimum Largest sum
- (875) KoKo
- (1011) D-day shipping

## Accumulative counting
With recursive logic.
- Recursive(DC/DFS)
  - (543) Diameter of BT
  - (50) Pow(x, n)
- DP  
  - (509) Fibonacci
  - (62, 63, 64) Unique Path
  - (70) Climbing stairs
  - (746) Climbing stairs II
  - (45) Jump game II
  - (198) House Rabber
  - (322) Coin Change

## Others
### processed counting
with PQ to process the result step by step
- (1046) Stone smash
- (787) Cheapest path
- (743) Network Delay
- (502) IPO


### subtractive
- (2055) Plates & candles

### calculation
- (1128) Domino pairs
- (447) Boomerang amount