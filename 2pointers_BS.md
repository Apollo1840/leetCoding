# Two pointers (Binary search)

```python
# HalfOpen: left-closed, right-open pattern

l, r = 0, len(s)
while l < r:
    mid = (l+r)//2
    # do something
    
    # update
    if ...:
        l = mid + 1
    elif ...:
        r = mid
    else:
        ...

```

    "BS needs practice."

- (Indoor) 35 insert
- 69	sqrt
- 34	First and Last
- (Hard) 153	Minimum in cycle

## BS Computing
Sometimes use tricky BS to find numerical solution.

- 410 Mininum largest sum
- 875 Koko


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


example:
```python

# 410 Minimum largest sum
def splitArray(self, nums: List[int], k: int) -> int:
        l, r = 0, sum(nums)
        while l < r:
            mid = (l +r)//2
            if self.achieve(nums, mid, k):
                # print("ok with {}".format(mid))
                r = mid
            else:
                l = mid + 1
        return l

def achieve(self, nums, target_sum, k):
    p1, p2 = 0, 0
    curr_sum = 0
    while p2 <= len(nums) and k>0:

        if p2 < len(nums):
            curr_sum += nums[p2]

        # print("p1: {}, p2:{}, curr_sum: {}".format(p1, p2, curr_sum))
        if curr_sum > target_sum:
            # print(target_sum, nums[p1:p2])
            p1 = p2
            k -= 1

            if p2 < len(nums):
                curr_sum = nums[p2]

        if curr_sum <= target_sum:
            p2 += 1
        

    return k > 0
        

# 875 Koko
def minEatingSpeed(self, piles: List[int], h: int) -> int:
    l, r = math.ceil(sum(piles)/h), max(piles)
    while l < r:
        mid = (l + r)//2
        if self.achieve(piles, mid, h):
            # print("ok with {}".format(mid))
            r = mid
        else:
            l = mid + 1
    return l

def achieve(self, piles, mid, h):
    total_time = 0
    for i in piles:
        total_time += math.ceil(i/mid)
        if total_time > h:
            break
    return total_time <= h





```