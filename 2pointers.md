# Two pointers

Trival case: two pointers on two arrays

- 392 Is subsequence
- 925 Is Long-pressed

```python
p1, p2 = 0, 0 
while p1<len(a) and p2<len(b):
# while p1<len(a): # mostly single track
    
    # ...
    p1+=1
    p2+=1
    
    # ...
    p1+=1

# return p2==len(b)

```