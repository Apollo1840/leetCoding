# Two pointers (Slow-Fast)

Conventionally, I use `p1` as slow pointer and `p2` as fast pointer.
`p1` also used to mark something until it meets new occurence and update itself as `p2`(`p1 <- p2`).


```python
p1, p2 = 0, 0   # Slow, Fast
while p2<len(a):
    
    if ...
        p1=p2
    
    # ...
    p2+=1
    
# example: distance to the left char
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

# (121 Best BuySale)
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


## In-place

Fast-slow two pointers often used in case where in-place modification is required.
- 26 remove duplicate

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


### Three pointers

Three pointers are:
- p1: Overlay, as the slowest pointer
- p2_1: Slow pointer, hold to count
- p2_2: Fast pointer, forward to look up.

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