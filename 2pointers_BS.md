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