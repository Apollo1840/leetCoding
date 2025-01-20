# Two pointers (Slow-Fast)

## In-place

Fast-slow two pointers often used in case where in-place modification is required.

Basic form:

```python
p1, p2 = 0, 0   # Slow, Fast
while p2<len(a):
    
    if ...
        a[p1] = ...
        p1+=1
    
    # ...
    p2+=1
    
# return p1
```

Examples:
- 26 remove duplicate


### Three pointers

Three pointers are:
- p1: Overlay, as the slowest pointer
- p2_1: Slow pointer, hold to count
- p2_2: Fast pointer, forward to look up.

Challenges:
- 443 Compress duplicates
- (Reverse) 88 In-place mergeSort

```python
        
# Compress duplicates: "aaabbc" -> "a3b2c"
def compress(self, chars: List[str]) -> int:
    p1, p2_1, p2_2 = 0, 0, 0  # overlay, slow, fast
    char_curr = chars[0]

    while p2_2 <= len(chars):
        # when it is over or meets different
        if p2_2 == len(chars) or chars[p2_2]!=char_curr:
            chars[p1] = chars[p2_1]
            p1 += 1

            if p2_2 - p2_1 > 1:
                num = str(p2_2 - p2_1)
                chars[p1:p1+len(num)] = num
                p1 += len(num)
            
            # slow catch up with fast
            p2_1 = p2_2

            # change current char if needed
            if p2_2 < len(chars):
                char_curr = chars[p2_2]

        # print(p1,p1_1, p1_2, chars[0:p1])
        p2_2 += 1
    chars = chars[0:p1]
    return len(chars)


# In-place mergeSort
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    """
    Two Pointers: Inverse 
    Acctually Three pointers:
    - p1_1: slow-1, as inverse pointer of 1st array
    - p1_2: slow-2, as inverse pointer of 2rd array
    - p2: fast pointer, as overlay index
    
    # PS. in merge case, overlay pointer is the fastest.
    """
    p1_1 = m - 1
    p1_2 = n - 1
    p2 = m + n - 1
    
    while p1_2 >= 0:

        # move p1_1 if nums1 is bigger (and when p1_1 is movable)
        if p1_1 >= 0 and nums1[p1_1] > nums2[p1_2]:
            nums1[p2] = nums1[p1_1]
            p1_1 -= 1
        
        # move p1_2 if nums2 is bigger
        else:
            nums1[p2] = nums2[p1_2]
            p1_2 -= 1

        p2 -= 1

        

```

## Sliding window

- (209) Shortest sub-array

## Jumping pointer
- (Jumping slow) 3 non-repeating sub-array
- (Jumping fast) 763 non-crossing sub-array

```python

# (3) non-repeating sub-array
def nonRepeating(self, s):
    visited = dict()  # non-repeating start
    res = 0
    
    p1 = -1
    for p2, c in enumerate(s):
        if c in visited:
            p1 = max(p1, visited[c])
        visited[c] = p2
        res = max(res, p2-p1)
    return res        
        

# (763) non-crossing sub-array
def partitionLabels(self, s: str) -> List[int]:
    last = dict()
    for i, c in enumerate(s):
        last[c] = i
    
    ans = []
    anchor = 0
    
    p2 = 0
    for p1, c in enumerate(s):
        p2 = max(p2, last[c])
        if p1 == p2:
            ans.append(p2 - anchor + 1)
            anchor = p1 + 1  
    return ans


```

### Step recorder

A typical jumping slow pointer is used as **step recorder**.

Conventionally, I use `p1` as slow pointer and `p2` as fast pointer.
`p1` also used to mark something until it meets new occurence and update itself as `p2`(`p1 <- p2`).

Basic form:

```python
p1, p2 = 0, 0   # Slow, Fast
while p2<len(a):
    
    if ...
        p1=p2
    
    # ...
    p2+=1
    
```

Examples:
- Smallest Larger
- Distance to LeftChar
- Best BuySale (121)
- Max sub-array (53)

**Smallest Larger**

Task: given nums and target, find the index of the smallest number in nums which is larger than target.

```python

p1, p2 = 0, 0
smallest = float('inf')
while p2 < len(nums):
    if target < nums[p2] < smallest:
        smallest = nums[p2]
        p1 = p2
    p2+=1
return p1

```

**Distance to LeftChar**

Task: given a string and a target char, return list of number counting char's distance to its left target char.

```python

def leftToChar(self, s, char):
    res = [float("Inf")]*len(s)
    p1, p2 = 0, 0  # p1 mark the left char
    start = False
    while p2 < len(s):
        if s[p2] == char:
            p1 = p2
            start = True
        if start:
            res[p2] = p2 - p1
        p2 += 1
    return res

```


**Best BuySale(121)**

```python
# example: maxProfit buy one time stock
def maxProfit(self, prices: List[int]) -> int:
    profit = 0
    p1, p2 = 0,0  # p1 mark the minPurchase
    while p2 < len(prices):
        if prices[p2] < prices[p1]:
            p1 = p2
        
        profit = max(profit, prices[p2] - prices[p1])
        p2+=1
    return profit

```
