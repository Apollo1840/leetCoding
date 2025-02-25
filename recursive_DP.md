# Dynamic Programming

## Prerequisites

Related to a concept called **Recursive Sequence**:

$$ a_n = f(a_{n-1}, a_{n-2}, ...)
$$

eg.

- $a_n = a_{n-1} + a_{n-2}$
- $a_n = a_{n-1} * a_{n-2}$
- $a_n = min(a_{n-1}, a_{n-2}+b_{n})$

Representative challenges:

- (indoor) (509) Fibonacci

## Ideaology

Dynamic Programming (DP) incrementally stores intermediate **results** in RAM, with each subsequent result being derived
from previously computed values (like what happened in **Recursive Sequence**), continuing until the final desired
result is obtained.

Since we are frequently re-use the previously computed values, it is beneficial to store those intermediate results in
RAM (array for index query, or dict for hash query).

(Notes: when you intutively get a recursive methods like DC or RC, think whether is able to transfer to DP to be more
efficient)

## Methodology

### Standard DP

Can be understood as single-channel DP, where an array of answers or answer-related array `dp` is maintained.

#### Trival DP

`dp[i]` based on `dp[i-1], dp[i-2], ..., dp[i-k]`:

- (509) Fibonacci (`dp[i] = dp[i-1] + dp[i-2]`)
- (70) Climbing stairs (`dp[i] = dp[i-1] + dp[i-2]`)

#### Jumping DP

`dp[i]` based on some `dp[i-x]`, where `x` is not straightforward)

- (338) Counting Bits (hint: forward generative)

#### Flexible DP

`dp[i]` based on a **dynamic** range of previous results `dp[i-1], dp[i-2], ...` and usually with multi-base:

- Conditional
    - (368) Divisible Subset (hint: DP not as result, but a processing tool)
    - (300) longest sub-increasing (based on all previous)
    - (673) \# longest sub-increasing (hint: double-DP)

Enumerate all previous indices and update on a sub-set based on certain condition.

```python
# longest sub-increasing
for j in range(i):
    if nums[j] < nums[i]:
        dp[i] = max(dp[i], dp[j] + 1)

# divisible subset (relaxation: only counting max size of the divisible subset)
for j in range(i):
    if nums[i] % nums[j] == 0:
        dp[i] = max(dp[i], dp[j] + 1)

        # use prev, max_index, max_length to store the information for trace back the results

```

- Conditional fixed-set
    - (322) Coin change
    - (279) Perfect Squares

Enumerate some previous indices and update on a sub-set based on certain condition.

```python

# coin change
for elem in coins:
    if i - elem >= 0:
        dp[i] = min(dp[i], dp[i - elem] + 1)

# perfect square
for elem in squares:
    if i - elem >= 0:
        dp[i] = min(dp[i], dp[i - elem] + 1)

# If coins and squares are many and sorted.
# we can use (if i-elem <0: break) to accelerate the algorithm
```

**(139) Word break**
Can be solved both with conditional method and Conditional fixed-set method.

```python

# word break: conditional
# (better approach)
for j in range(max(i - wordlen_max, 0), i):
    if s[j:i] in wordDictSet:
        dp[i] = dp[i] or dp[j]
        if dp[i]:
            break

# word break: conditional fix-set
for elem in wordDict:
    if i - len(elem) >= 0 and s[i - len(elem):i] == elem:
        dp[i] = dp[i] or dp[i - len(elem)]
        if dp[i]:
            break

```

### Multi-channel DP

Multiple DP sequences are adapted.

Either in form of ：

- `dp_0`, `dp_1` or
- `dp[:][0]`, `dp[:][1]` or
- `dp[i][j]` as 2-D DP

If the number of DP arrays are fixed and equiped with specfic meaning, then recommended to use meaningful name such
as `dp_name_1`, `dp_name_2`.

Examples:

- (256) Paint House
- (673) \# longest sub-increasing (hint: conditional flexible DP)

```python

#  Paint House
for i in range(1, n):
    dp_red[i] += min(dp_green[i - 1], dp_blue[i - 1])
    dp_green[i] += min(dp_red[i - 1], dp_blue[i - 1])
    dp_blue[i] += min(dp_red[i - 1], dp_green[i - 1])
# (p.s here DP is also alternative)

# Number of longest sub-increasing
for j in range(i):
    if nums[j] < nums[i]:
        if dp_len[j] + 1 > dp_len[i]:
            dp_len[i] = dp_len[j] + 1
            dp_count[i] = dp_count[j]
        elif dp_len[j] + 1 == dp_len[i]:
            dp_count[i] += dp_count[j]
```

#### High dimensional DP

High dimensional DP is a special case of Multiple DP. Usually using `dp[i][j]` as 2-D DP.

- (2D native) **Matrix** operation (On Board)
    - Naive-Board
      - (simple) (62) Unique Path
      - (63) Unique Path II
    - 01-Board
      - (542) Closest zero
      - (221) Maximum Square
    - Number-Board
      - (329) Increasing Path
- **String** operation
    - single
        - (2D) (5) Longest Sub-Palindromic
    - tuple
        - (2D) (97) Interleaving string
        - (2D) (72) Edit distance
        - (2D) (LCS) (1143/583)Longest common sub-seq/Delete game
        - (2D) (LCS) (718) Longest commong sub-array

**(5) Longest sub-Palindromic**
Task: find longest Plindromic substring given a string.

Hint: use a 2-D dp matrix to record `s[i:(j+1)]` **is Palindrome or not**

Solution: `dp[i][j] = dp[i+1][j-1] and s[i] == s[j]`

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
                    start, end = i, j  # update to the longest
    return s[start: (end + 1)]

```

**(97) Interleaving string**
Task: judge whether two string can interleave a target string or not.

Hint: `dp[i][j]` represent whether `s1[:i]`, `s2[:j]` interleave `s3[:(i+j)]`

Solution:

```python

dp[i][j] = (dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]) or
           (dp[i][j - 1] and s2[j - 1] == s3[i + j - 1])

```

**(72) Edit distance**
Task: count minimum number of operation that makes word1 and word2 same.

Hint: use `dp[i][j]` to represent minimum number of operations that makes `word1[:i]` and `word2[:j]` same.

Solution:

```python
if word1[i - 1] == word2[j - 1]:
    dp[i][j] = dp[i - 1][j - 1]
else:
    dp[i][j] = 1 + min(
        dp[i - 1][j],  # Delete from word1 (delete word1[i-1])
        dp[i][j - 1],  # Insert into word1 (from word2[j-1])
        dp[i - 1][j - 1]  # Replace in word1 (word1[i-1] <- word2[j-1])
    )
```

**(1143/583) LCS/Delete game**
Task:

- 1143: count length of the longest common sub sequence.
- 583: count minimum number of deletion that makes word1 and word2 same.

Hint: use `dp[i][j]` to represent length of longest equal sub-sequence between `word1[:i]` and `word2[:j]`.
`if word1[i-1] == word2[j-1]: dp[i][j] = dp[i-1][j-1] + 1`

```python

def minDistance(self, word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    # dp[i][j] represents the LCS length between word1[:i] and word2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # LCS length
    lcs = dp[m][n]
    # Minimum deletions = total characters - 2 * LCS length
    return m + n - 2 * lcs
```

**(718) LCS sub-array**
Task:  count length of the longest common sub-array.

Hint: `dp[i][j]` represents length of LCS between `nums1[:i]` and `nums2[:j]` ends with `nums1[i-1]` and `nums2[j-1]`.

```python

def findLength(self, nums1: List[int], nums2: List[int]) -> int:
    """
    dp[i][j]:  LCS between nums1[:i] and nums2[:j] ends with nums1[i-1] and nums2[j-1]
    """
    m, n = len(nums1), len(nums2)
    max_len = 0
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if nums1[i - 1] == nums2[j - 1]:
                dp[i][j] = max(dp[i][j], dp[i - 1][j - 1] + 1)
            else:
                dp[i][j] = 0
            max_len = max(max_len, dp[i][j])
    return max_len


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

### Multiple-rounds DP

- (821) closest occurency
- (2D) (542) closest zero

#### Flexible multi-rounds DP

- (518) Coin change II
- (416) Coin change III -> Equal partition

**Coin change II**
Task: count the number of combinations of coins sum up to target(amount)

Idea: `dp[i]` stores number of combinations that can sum up to `i`.

Solution: 

```python
for c in coins:
    for i in range(c, amount):
        dp[i] += dp[i-c]
```

**Coin change III**
Task: judge whether sub-sequence of coins can sum up to the target. (Note that each coin can be used only once in the coins bag.)

Idea: `dp[i]` stores whether coins can sum up to `i`.

Solution: 

```python
for coin in coins:
    # Iterate backwards to ensure each coin is only used once
    for i in range(amount, coin - 1, -1):
        dp[i] |= dp[i - coin]

```

**Equal partition**
Task: return whether can split the array into two subsets with equal sum.

Idea: calculate half-sum, then the problem = Coin Change III. we can use `dp[i]` to store whether can get a sub-sequence that can sum up to i.


### Aligned DP

`dp[i]` based on `dp[i-1], dp[i-2], ...` and an extra sequence `a[i]`:

- (746) Climbing stairs II
- (198) House Robber

#### Alternative DP

Based on `dp[i-1]` and others, but for the easy of thinking.

- (55) Jump game
- (45) Jump game II

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
        if i > dp[i - 1]:  # If we can't reach this index, return False
            return False
        dp[i] = max(dp[i - 1], i + nums[i])  # Update max reach
        if dp[i] >= n - 1:  # Early stopping condition
            return True

    return dp[n - 1] >= n - 1

```

**Jump game II**
Task: Minimum number of jumps reach the top.

Idea: Use DP to store answer.

```python

def jump(self, nums: List[int]) -> int:
    # remove n==0:

    # initiate dp
    n = len(nums)
    dp = [(float("Inf"))] * (n + 1)

    # start point
    dp[0] = 0

    # accumulate:
    power = 0
    power_remain = 0
    for i in range(1, n):
        # print("dp", dp[:i])
        power = max(power, nums[i - 1])
        if power_remain > 0:
            dp[i] = dp[i - 1]
            power_remain -= 1
        else:
            dp[i] = dp[i - 1] + 1
            power_remain = power - 1
        power -= 1

    return dp[n - 1]

```

Comments: DP is not necessary here, because only `dp[i-1]` is used

### Coin change

- (322) Coin change: fewest \# of coins
- (518) Coin change II: \# of combinations

```python
# Task: minimum number of coins
    # ...
for c in coins:
    if i - c >= 0:
        dp[i] = min(dp[i], dp[i - c] + 1)
# ...
```

```python
# Task: number of combinations
    # ...
for c in coins:
    for i in range(c, amount):
        dp[i] += dp[i-c]
# ...
```





