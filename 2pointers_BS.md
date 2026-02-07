# Two pointers (Binary search)

```python
# HalfOpen: left-closed, right-open pattern

l, r = 0, len(s) - 1
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

Examples:
- (Indoor) 35 insert
- 69	sqrt
- 34	First and Last

## In Rotated Array
- 153   Minimum in rotated
- 33    Search in rotated
- 81    Search in rotated II

In this cases, we usually differentiate the situation via:
- edge case
- left part is sorted  (`nums[left] <= nums[mid]`)
- right part is sorted (`else`)

```python

# 153 Minimum in rotated
def findMin(self, nums: List[int]) -> int:
    l, r = 0, len(nums)-1
    while l < r:
        mid = (l+r)//2

        # End case
        if nums[l] == nums[mid]:
            break
        
        # Edge case
        if nums[l] < nums[r]:
            return nums[l]
        
        # left part is sorted
        if nums[l] < nums[mid]:
            l = mid + 1
        
        # right part is sorted
        else:
            r = mid

    return min(nums[l], nums[r])

# 81 Search in rotated II
def search(self, nums: List[int], target: int) -> bool:
    l, r = 0, len(nums) - 1

    while l < r:
        mid = (l + r) // 2

        # End case: check if the middle element is the target
        if nums[mid] == target:
            return True

        # Edge cases: Handle duplicates
        if nums[l] == nums[mid] == nums[r]:
            l += 1
            r -= 1

        # Left part is sorted
        elif nums[l] <= nums[mid]:
            if nums[l] <= target < nums[mid]:
                r = mid
            else:
                l = mid + 1

        # Right part is sorted
        else:
            if nums[mid] < target <= nums[r]:
                l = mid + 1
            else:
                r = mid

    return nums[l]==target  # l == mid == r

```

## BS Computing
Sometimes use tricky BS to find numerical solution.

- 410 Mininum largest sum
- 875 Koko
- 1011 D-day shipping


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


Examples:
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
    curr_sum = 0  # Tracks the sum of the current segment

    for num in nums:
        curr_sum += num

        # If the current sum exceeds the target, we need to start a new segment
        if curr_sum > target_sum:
            k -= 1
            curr_sum = num  # Start a new segment with the current number

            # If `k` drops below zero, it means we cannot split the array as required
            if k < 0:
                return False

    return True  # If we complete the loop with k >= 0, it's achievable
        

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


# 1011 D-day shipping
def achieve(self, weights, capacity, d):
    """
    same achieve function as in (410 Minimum largest sum)
    """
    curr_sum = 0  # Tracks the sum of the current segment

    for weight in weights:
        curr_sum += weight

        # If the current sum exceeds the target, we need to start a new segment
        if curr_sum > capacity:
            d -= 1
            curr_sum = weight  # Start a new segment with the current number

            # If `k` drops below zero, it means we cannot split the array as required
            if d < 0:
                return False

    return True  # If we complete the loop with k >= 0, it's achievable
        


```

## BS as Problem Reduce/DC
Very similar to the idea of Divide&Conquer(DC).

- 162 Local Maximum

Half the problem size.

- (Hard) 4 Median in 2 sorted