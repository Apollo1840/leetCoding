# Dynamic Programming

Store gradually increased intermediate results.

Related to a concept called **Recursive Sequence**:

$$
    a_n = f(a_{n-1}, a_{n-2}, ...)
$$

eg.

- $a_n = a_{n-1} + a_{n-2}$
- $a_n = a_{n-1} * a_{n-2}$
- $a_n = min(a_{n-1}, a_{n-2}+b_{n})$


Challenges:
- (indoor) (509) Fibonacci


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

Use DP to store **max reachable index over than index** instead of the answer upto i.

**Jump game**
Task: Whether able to reach top.


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

Use DP to store answer.

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
use `dp[i-1], dp[i-2], ..., dp[i-k]`:

- (509) Fibonacci
- (70) Climbing stairs

### Flexible DP
use dynamic number of previous results `dp[i-1], dp[i-2], ...`:
- (322) Coin change

### Standard DP
use extra sequence `a[i]` and `dp[i-1], dp[i-2], ...`:
- (746) Climbing stairs II
- (198) House Robber

### Alternative DP
Based on `dp[i-1]` and others, but for the easy of thinking.

- (55) Jump game
- (45) Jump game II

### Multiple-rounds DP
- (821) closest occurency
- (2D) (542) closest zero

### High dimensional DP
- (2D native) (simple) (62) Path Number
- (2D native) (63) Unique Path II 
- (2D native) (542) 0-1 Matrix
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





