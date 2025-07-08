# Two pointers

Trival case: two pointers on two arrays

- 392 Is subsequence
- 925 Is Long-pressed

```python
# suppose we have two arrays: a1, a2

p1, p2 = 0, 0 
while p1<len(a1) and p2<len(a2):
# while p1<len(a1): # mostly single track
    
    # ...
    p1+=1
    p2+=1
    
    # ...
    p1+=1

# return p2==len(a2)

```