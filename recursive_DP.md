# Dynamic Programming

## Prerequisites

Related to a concept called **Recursive Sequence**:

$$
    a_n = f(a_{n-1}, a_{n-2}, ...)
$$

eg.

- $a_n = a_{n-1} + a_{n-2}$
- $a_n = a_{n-1} * a_{n-2}$
- $a_n = min(a_{n-1}, a_{n-2}+b_{n})$

Representative challenges:
- (indoor) (509) Fibonacci


## Ideaology
Dynamic Programming (DP) incrementally stores intermediate **results** in RAM, 
with each subsequent result being derived from previously computed values (like what happened in **Recursive Sequence**), 
continuing until the final desired result is obtained.

Since we are frequently re-use the previously computed values, 
it is beneficial to store those intermediate results in RAM (array for index query, or dict for hash query).

(Notes: when you intutively get a recursive methods like DC or RC, think whether is able to transfer to DP to be more efficient)


## Classic
### Climbing stairs
Distintic ways to reach top.

- (70) Climbing stairs
- (746) Climbing stairs II
- (198) House Robber

**Climbing stairs**
Task: #distintive ways to reach top.

Solution: `dp[i] = dp[i-1] + dp[i-2]`

**Climbing stairs II**
Task: minium cost to reach top.

Solution: `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`

**House Robber**
Task: maxmium money get from non-adjacent house.

Solution: `dp[i] = max(dp[i-2] + nums[i-1], dp[i-1])`

### Jump game
Whether able to reach top.

- (55) Jump game
- (45) Jump game II

**Jump game**
Task: Whether able to reach top.

Idea: Use DP to store **max reachable index over than index** instead of the answer upto i.

Solution: `dp[i] = max(dp[i-1], i + nums[i])`

```python
def jump(nums):
    n = len(nums)
    if n == 1:
        return True  # If there's only one element, we're already at the end.

    dp = [0] * n  # dp[i] stores the max index we can reach from index `i`
    dp[0] = nums[0]  # Initially, the max we can reach is nums[0]

    for i in range(1, n):
        if i > dp[i-1]:  # If we can't reach this index, return False
            return False
        dp[i] = max(dp[i-1], i + nums[i])  # Update max reach
        if dp[i] >= n - 1:  # Early stopping condition
            return True

    return dp[n-1] >= n - 1

```

**Jump game II**
Task: Minimum number of jumps reach the top.

Idea: Use DP to store answer.

```python

def jump(self, nums: List[int]) -> int:
    # remove n==0:

    # initiate dp
    n = len(nums)
    dp = [(float("Inf"))] * (n+1)

    # start point
    dp[0] = 0

    # accumulate:
    power = 0
    power_remain = 0
    for i in range(1, n):
        # print("dp", dp[:i])
        power = max(power, nums[i-1])
        if power_remain > 0:
            dp[i] = dp[i-1]
            power_remain -= 1
        else:
            dp[i] = dp[i-1] + 1
            power_remain = power - 1
        power -= 1

    return dp[n-1] 

```


Comments: DP is not necessary here, because only `dp[i-1]` is used

### Coin change
Fewest number of coins to make up the amount.

- (322) Coin change

```python
    # ...
    for c in coins:
        if i-c >=0:
            dp[i] = min(dp[i], dp[i-c] + 1)
    # ...
```

## Methodology

### Trival DP
`dp[i]` based on `dp[i-1], dp[i-2], ..., dp[i-k]`:

- (509) Fibonacci
- (70) Climbing stairs

**Jumping DP**
- (338) Counting Bits
- (368) Divisible Subset

### Flexible DP
`dp[i]` based on a **dynamic** range of previous results `dp[i-1], dp[i-2], ...`:
- (322) Coin change
- (139) Word break

```python

# coin change
for c in coins:
    if i-c >=0:
        dp[i] = min(dp[i], dp[i-c] + 1)
        

# word break
for word in wordDict:
    if i-len(word) >= 0:
        if dp[i-len(word)] and s[i-len(word):i] in word_set:
            dp[i] = True
            break
```

- (300) longest sub-increasing (based on all previous)


### Standard DP
`dp[i]` based on `dp[i-1], dp[i-2], ...` and an extra sequence `a[i]`:
- (746) Climbing stairs II
- (198) House Robber


### Alternative DP
Based on `dp[i-1]` and others, but for the easy of thinking.

- (55) Jump game
- (45) Jump game II


### Multiple-rounds DP
- (821) closest occurency
- (2D) (542) closest zero

#### Flexible multi-rounds DP
- (416) Equal partition

**Equal partition**
Task: return whether can split the array into two subsets with equal sum.

Idea: calculate half-sum, use `dp[i]` to store whether can get subsum as i.

Solution: `dp[i] = dp[i-num]`(if num not used)

### High dimensional DP
- (2D native) (simple) (62) Unique Path
- (2D native) (63) Unique Path II 
- (2D native) (542) Closest zero
- (2D native) (329) Increasing Path
- (2D) Longest Sub-Palindromic 

**Longest sub-Palindromic**
Task: find longest Plindromic substring given a string.

Solution: use a 2-D dp matrix to record **s[i:(j+1)] is Palindrome or not**

Hint: `dp[i][j] = dp[i+1][j-1] and s[i] == s[j]`

```python
def longestPalindrome(self, s: str) -> str:
    n = len(s)
    # DP table to check if s[i:j] is a palindrome
    dp = [[False] * n for _ in range(n)]
    
    start, end = 0, 0
    # Fill DP table
    for length in range(n):  # Length of substring
        for i in range(n - length):
            j = i + length
            if s[i] == s[j]:
                if length <= 1:  # Single character or two equal characters
                    dp[i][j] = True
                else:
                    dp[i][j] = dp[i + 1][j - 1]  # Check inner substring
                if dp[i][j]:
                    start, end = i, j
    return s[start: (end+1)]

```

### Hashmap DP
- (1025) Divisor Game

```python

def dp(n):
    if n in self.dp_map:
        return self.dp_map[n]
    
    # res = = self.calc(self.dp(n-1), ...)
    self.dp_map[n] = res
    return self.dp_map[n]

```




