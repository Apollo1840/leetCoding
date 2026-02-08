# Dynamic Programming

## Prerequisites

Related to a concept called **Recursive Sequence**:

$$ a_n = f(a_{n-1}, a_{n-2}, ...)
$$

eg.

- $a_n = a_{n-1} + a_{n-2}$
- $a_n = a_{n-1} * a_{n-2}$
- $a_n = \min(a_{n-1}, a_{n-2}+b_{n})$

Representative challenges:

- (indoor) (509) Fibonacci

## Ideaology

Dynamic Programming (DP) incrementally stores intermediate **results** in RAM, with each subsequent result being derived
from previously computed values (like what happened in **Recursive Sequence**), continuing until the final desired
result is obtained.

Since we are frequently re-use the previously computed values, it is beneficial to store those intermediate results in
RAM (array for index query, or dict for hash query).

(Notes: when you intutively get a recursive methods like DC or RC, think whether is able to transfer to DP to be more memory
efficient)

## Methodology

### 1. Standard DP

Can be understood as single-channel DP, where an array of answers or answer-related array `dp` is maintained.

#### Trival DP

`dp[i]` based on `dp[i-1], dp[i-2], ..., dp[i-k]`:

- (509) Fibonacci (`dp[i] = dp[i-1] + dp[i-2]`)
- (70) Climbing stairs (`dp[i] = dp[i-1] + dp[i-2]`) (#Ways)



#### Aligned DP

`dp[i]` based on `dp[i-1], dp[i-2], ...` and an extra sequence `{a[i]}`:

- (746) Climbing stairs II
- (198) House Robber 
- (91) decode ways
```python
dp[i] += dp[i - 2]*(self.decodable(s[i - 2], s[i - 1])) + dp[i - 1]*(s[i - 1] != "0")
```

#### Conditional DP

`dp[i]` based on a **dynamic** subset of previous results `dp[i-1], dp[i-2], ...` and usually with multi-base.

**Bounded Condition**

Enumerate some previous indices and update on a sub-set of a fixed length.



With a subset as `{dp[i - elem]}`:

```python

# for i in range(n):
for elem in elements:
    if i - elem >= 0:
        dp[i] = ... dp[i - elem] ...
```

Examples:

```python

# (322) coin change
for elem in coins:
    if i - elem >= 0:
        dp[i] = min(dp[i], dp[i - elem] + 1)
        
# (279) perfect square
for elem in squares:
    if i - elem >= 0:
        dp[i] = min(dp[i], dp[i - elem] + 1)
        
# (377) coin change II+
for elem in coins:
    if i - elem >= 0:
        dp[i] += dp[i - elem]

# If coins and squares are many and sorted.
# we can use (if i-elem <0: break) to accelerate the algorithm

```

**Unbounded condition**

Enumerate all previous indices and update on a sub-set based on certain condition.
Like:

```python
# for i in range(n):
for j in range(i):
    if ...:
        dp[i] = ... dp[j] ...

```

Examples:

```python
# (300) longest sub-increasing
# Idea: `dp[i]` represents length of longest increasing subsequence ends with i.
for j in range(i):
    if nums[j] < nums[i]:
        dp[i] = max(dp[i], dp[j] + 1)

# (1025) Divisor Game
# Idea: `dp[i]` represents definite win of the next player. So fails into definite loss is a definite win.
for j in range(1, i):
     if i % j == 0:
        dp[i] = dp[i] or not dp[i-j]  # has more effective way.


# (368) divisible subset (relaxation: only counting max size of the divisible subset)
for j in range(i):
    if nums[i] % nums[j] == 0:
        dp[i] = max(dp[i], dp[j] + 1)

        # use prev, max_index, max_length to store the information for trace back the results


```


**(139) Word break**

Task: judge whether a long string can be divided into words in wordDict.

Hint: Can be solved either with **Bounded** or **Unbounded** method.

```python

# word break: Bounded
for elem in wordDict:
    if i - len(elem) >= 0 and s[i - len(elem):i] == elem:
        dp[i] = dp[i] or dp[i - len(elem)]
        if dp[i]:
            break

# word break: Unbounded
# (better approach)
for j in range(max(i - wordlen_max, 0), i):
    if s[j:i] in wordDictSet:
        dp[i] = dp[i] or dp[j]
        if dp[i]:
            break
```


To summarize:

- Bounded
    - (322/279) Coin change/Perfect Squares
    - (377) Coin change II+/Combination Sum

- Unbounded
    - (300) longest sub-increasing (hint: based on all previous)
    - (673) \# longest sub-increasing (hint: double-DP)
    - (368) Divisible Subset (hint: DP not as result, but a processing tool)

#### Jumping DP

`dp[i]` based on some `dp[i-x]`, where `x` is not straightforward:

- (338) Counting Bits (`dp[i] = dp[i - i//2] + i%2` )(hint: understand it with forward generative)

### 2. Multi-channel DP (2D DP)

Multiple DP sequences are adapted.

Either in form of ：

- `dp_0`, `dp_1` or
- `dp[:][0]`, `dp[:][1]` or
- `dp[i][j]` as 2D DP

If the number of DP arrays are fixed and equiped with specfic meaning, then recommended to use meaningful name such
as `dp_name_1`, `dp_name_2`.

Examples:

- (256) Paint House
- (673) \# longest sub-increasing (hint: Unbounded DP, based on all previous)

**(256) Paint House**

Idea: `dp_color[i]` represents minimum cost of painting houses with `i`-th room paint as `color`.

```python
# (!) First initialize dp_red, dp_green, dp_blue with sole cost.

for i in range(1, n):
    dp_red[i] += min(dp_green[i - 1], dp_blue[i - 1]) + x_red
    dp_green[i] += min(dp_red[i - 1], dp_blue[i - 1]) + x_green
    dp_blue[i] += min(dp_red[i - 1], dp_green[i - 1]) + x_blue
# (p.s here DP is also alternative)

```

**(673) \# longest sub-increasing**

Idea:

- `dp_len[i]`  represents the **length** of longest increasing subsequence ends with `nums[i]`.
- `dp_count[i]` represents the **amount** of longest increasing subsequence ends with `nums[i]`.

Solution:

```python

for i in range(n):
    for j in range(i):
        if nums[j] < nums[i]:

            # update dp_count 
            if dp_len[j] + 1 > dp_len[i]:
                dp_count[i] = dp_count[j]
            elif dp_len[j] + 1 == dp_len[i]:
                dp_count[i] += dp_count[j]

            # update dp_len
            dp_len[i] = max(dp_len[i], dp_len[j]+1)

```

#### 2-dimensional(2D) DP

Using `dp[i][j]` as 2-D DP. Use case including:

- **Matrix** operation and
- **String** operation.
- others

Challenges:

##### 1) Matrix (Or Board)

**(62) Unique Path**
Task: count number of unique paths from left-upper to right-bottom.

Solution: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

**(221) Maximum square**
Task: return the area of the maximum ones square

Idea: `dp[i][j]` represents largest area of matrix[:i][:j] with largest square right-bottom corner align to the matrix's right-bottom corner.
        
Solution:

```python

if matrix[i - 1][j - 1] == "1":
    dp[i][j] = 1 + min(dp[i - 1][j],
                       dp[i][j - 1],
                       dp[i - 1][j - 1]) # for the left top corner
    max_side = max(max_side, dp[i][j])
else:
    dp[i][j] = 0

```

Summary: 
- Naive-Board
    - (simple) (62) Unique Path
    - (63) Unique Path II
- 01-Board
    - (542) Closest zero
    - (221) Maximum Square
- Number-Board
    - (329) Increasing Path

##### 2) String

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

**(718) LCS**
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
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = 0
            max_len = max(max_len, dp[i][j])
    return max_len


```


**(1143) LCS sub-seq**

Task: Count length of the longest common sub sequence.


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
    return dp[m][n]
```

**(583) Delete game**: count minimum number of deletion that makes word1 and word2 same.

```python

    lcs = dp[m][n]
    # Minimum deletions = total characters - 2 * LCS length
    return m + n - 2 * lcs
```


**(5) Longest sub-Palindromic**
Task: (relaxtion) find the length of longest Plindromic substring given a string.

Hint: use a 2-D dp matrix to record `s[i:(j+1)]` **is Palindrome or not**. Unbounded DP with DP as a process, not the result. 

Solution: `dp[i][j] = dp[i+1][j-1] and s[i] == s[j]`

```python

def lengthOflongestPalindrome(self, s: str) -> int:
    n = len(s)
    # DP table to check if s[i:j] is a palindrome
    dp = [[False] * n for _ in range(n)]

    max_len = 0
    
    # Fill DP table
    for j in range(n):
        for i in range(j + 1): 
            if s[i] == s[j] and (j - i <= 1 or dp[i + 1][j - 1]):
                dp[i][j] = True
                max_len = max(max_len, j-i+1)
    return max_len

```

**(97) Interleaving string**
Task: judge whether two string can interleave a target string or not.

Hint: `dp[i][j]` represent whether `s1[:i]`, `s2[:j]` interleave `s3[:(i+j)]`

Solution:

```python

dp[i][j] = (dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]) or
(dp[i][j - 1] and s2[j - 1] == s3[i + j - 1])

```

Summary:

- tuple
    - (ED (72) Edit distance
    - (LCS) (718) Longest common sub-array
    - (LCS) (1143) Longest common sub-seq
      - (583) Delete game
    - (97) Interleaving string
    - (115) \# sub-sequences
    
- single
    - (5) Longest Sub-Palindromic

##### 3) others

(1155) Dice Rolls With Target Sum



### 3. Multiple-rounds DP

#### Two rounds DP

  - (821) closest occurency
  - (2D) (542) closest zero

#### Multi-rounds DP

Also can be understood as a generative way.

(Highly recommended to learn [classic: Coin change](#coin-change) first.)

- (518) Coin change II
- (474) Coin change III+ -> Ones and zeros (hint: 2D DP)
- (416) Coin change IV -> Equal partition
- (494) Coin change V -> Target Sum

**0/1 Knapsack**
Task:  The goal is to determine the optimal way to pack the backpack to maximize the total value while ensuring the total weight does not exceed m.
Given n items and a backpack with a maximum weight capacity of m, each item has a weight W[i] and a value V[i].


Idea: 
- `dp[i]` represents max value can achieve with capacity limit as `i`.
- Similar to Coin Change III+.

Solution:

```python

for i in range(n):
    for j in range(m, W[i] - 1, -1):  # 逆序遍历，保证只用上一行的数据
        dp[j] = max(dp[j], dp[j - W[i]] + V[i])


```


**(474) Ones \& Zeros**
Task: return size of largest subset of strs where 0s and 1s below m and n.

Idea:

- `dp[i][j]` represents size of largest subset of strs where 0s and 1s below i and j.
- the solution is very similar to **Coin change III+**.

Solution:

```python

for s in strs:
    zeros = s.count('0')
    ones = s.count('1')
    # Update dp array in reverse to avoid reuse of the same string
    for i in range(m, zeros - 1, -1):
        for j in range(n, ones - 1, -1):
            dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)

```

**(416) Equal partition**
Task: return whether can split the array into two subsets with equal sum.

Idea: 
- A variant of Coin Change IV. Calculate half-sum, then the problem = Coin Change IV.
- `dp[i]` to store whether can get a sub-sequence that can sum up to i.

**(494) Target Sum**
Task: return number of combinations of expressions(+/-) results in target sum.

Idea: 
- A variant of Coin Change V. == "Count subsets of `[2*n for n in nums]` that sum to `target + sum(nums)`".
- `dp[i]` stores number of sub-sequences that can sum up to `i - sum(nums)`. So the answer is `dp[target * sum(nums)]`. Each `n` contribute `2n` to the sum.

```python

def findTargetSumWays(self, nums: List[int], target: int) -> int:
    """
    dp[i] represent # expressions gets [i-sum(nums)]
    dp[i]
    """
    s = sum(nums)
    if target < -s or target > s:
        return 0

    dp = [0] * (2 * s + 1)
    dp[0] = 1
    for n in nums:
        for i in range(2*s, 2*n-1, -1):
            dp[i] += dp[i-2*n]
    return dp[target+s]

```

### Extra 1: Alternative DP

Based on `dp[i-1]` and others. Not necerssary regard as DP, but for the easy of thinking.

- (55) Jump game
- (45) Jump game II


### Extra 2: Hashmap DP

- (464) Addition game

```python

def dp(n):
    if n in self.dp_map:
        return self.dp_map[n]

    # res = = self.calc(self.dp(n-1), ...)
    self.dp_map[n] = res
    return self.dp_map[n]

```

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

There are two types of coin change problem:

- infinite coins: coins are given as type, and you can use as many that value of coin as you wish.
- finite coins (coins bag): each coin is unique, but might have same value. You can use each coin only once. This case
  is exactly knackspack problem.

Based those two categories:

- infinite coins
    - (322) Coin changeI: fewest \# of coins.
    - Coin change I+: largest \# of coins.
    - Coin change II*: judge exist combination.
    - (518) Coin change II: count \# of combinations
    - (377) Coin change II+: count \# of permutations(un-ordered combinations)
- finite coins
    - Coin change III: fewest \# of coins.
    - Coin change III+: largest \# of coins.
    - Coin change IV: judge exist sub-sequence.
    - Coin change V: count \# of sub-sequences.

#### Infinite coins

**Coin change I/I+**

Task: use fewest coins to make up total value M.

Idea:

- Use 1:      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
- Use 1,2:    -, -, 1, 1+1, 2, 2+1, 3, 3+1, 4, 4+1, 5
- Use 1,2,5:  -, -, -, -, -, 1, 1+1, 1+1, 1+2, 1+3, 2 

Solution:

```python

# Task: minimum number of coins.
# ...
for i in range(c, amount+1):
    for c in coins:
        if i - c >= 0:
            dp[i] = min(dp[i], dp[i - c] + 1)

# or
for c in coins:
    for i in range(c, amount):  
        dp[i] = min(dp[i], dp[i - c] + 1)

# print(dp) suppose coins = [1,2,5], amount = 5
# [1, 2, 3, 4, 5, 6]  only use coins = [1]
# [1, 1, 2, 2, 3, 3]  only use coins = [1, 2]
# [1, 1, 2, 2, 1, 2]  only use coins = [1, 2, 5]

# ...

# Task: maximum number of coins.
# ...
for i in range(c, amount+1):
    for c in coins:
        if i - c >= 0:
            dp[i] = max(dp[i], dp[i - c] + 1)
# ...
```

**Coin change II\***
Task: judge exist (ordered) combination.

Solution:

```python
# ...
for i in range(c, amount+1):
    for c in coins:
        if i - c >= 0:
            dp[i] |= dp[i - c]
# ...
```

**(518) Coin change II**

Note: `(1,2,1)` and `(2,1,1)` are same combinations.

Task: count the number of combinations of coins sum up to target(amount)

Idea: `dp[i]` stores number of combinations that can sum up to `i`.

Solution:

```python
# inverse from i-loop to c-loop
for c in coins:  
    for i in range(c, amount):
        dp[i] += dp[i - c]

    # print(dp) suppose coins = [1,2,5], amount = 5
    # [1, 1, 1, 1, 1, 1]  only use coins = [1]
    # [1, 1, 2, 2, 3, 3]  only use coins = [1, 2]
    # [1, 1, 2, 2, 3, 4]  only use coins = [1, 2, 5]

```

**(377) Coin change II+/Combination Count**

Also called count of **permutations**.

Note: `(1,2,1)` and `(2,1,1)`  are different (sequential) combinations.

Task: count the number of (sequential) combinations of coins sum up to target(amount)

Idea: `dp[i]` stores number of (sequential) combinations that can sum up to `i`.

Solution:

```python
# just switch the c-loop and i-loop, because we can use different coins at every step.
for i in range(1, amount + 1):
    for c in coins:
        if i - c >= 0:
            dp[i] += dp[i - c]
```

#### Finite coins

- Coin change III: fewest \# of coins.
- Coin change III+: largest \# of coins.
- Coin change IV: judge exist sub-sequence.
- Coin change V: count \# of sub-sequences.

**Coin change III/III+**
Task: return length of the shortest sub-sequence.

Idea: `dp[i]` stores length of the sub-sequence that can sum up to `i`.

Solution:

```python

# important initialization
dp[0] = 0

# Task: shortest
for c in coins:
    # Iterate backwards to ensure each coin is only used once
    for i in range(amount, c - 1, -1):
        dp[i] = min(dp[i], dp[i - c] + 1)

    # print(dp) suppose coins = [1, 1, 2, 4], amount = 5
    # [0, 1, inf, inf, inf, inf] used coins = [1]
    # [0, 1, 2, inf, inf, inf]   used coins = [1, 1]
    # [0, 1, 1, 2, 3, inf]       used coins = [1, 1, 2]
    # [0, 1, 1, 2, 1, 2]         used coins = [1, 1, 2, 4]

# Task: longest
for c in coins:
    for i in range(amount, c - 1, -1):
        dp[i] = max(dp[i], dp[i - c] + 1)
```



Similar challenge:
- (1049) Last stone

**Coin change IV**
Task: judge whether exist sub-sequence of coins can sum up to the target.

Idea: `dp[i]` stores whether exist sub-sequence can sum up to `i`.

Solution:

```python

for coin in coins:
    for i in range(amount, coin - 1, -1):
        dp[i] |= dp[i - coin]

    # print(dp) suppose coins = [1, 1, 2, 4], amount = 4
    # [T, T, False, False, False]   used coins = [1]
    # [T, T, T,     False, False]   used coins = [1, 1]
    # [T, T, T,     T, T]       used coins = [1, 1, 2]

```

Similar challenge:
- (416) Equal partition

**Coin change V**
Task: return number of distinct sub-sequences can sum up to the target.

Idea: `dp[i]` stores number of sub-sequences that can sum up to `i`.

Solution:

```python

for c in coins:
    for i in range(amount, c - 1, -1):
        dp[i] += dp[i - c]

```


Similar challenge:
- (494) Target Sum




