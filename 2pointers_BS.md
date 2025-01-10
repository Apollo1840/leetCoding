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

It needs practice.

- (Indoor) 35 insert
- 69	sqrt
- 34	First and Last
- (Hard) 153	Minimum in cycle

## BS Computing
Sometimes use tricky BS to find numerical solution.

- 410 Mininum largest sum
- 875 Koko