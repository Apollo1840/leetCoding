# Two pointers (Left-Right)

```python
l, r = 0, len(s)
while l < r:
    # do something
    
    # update
    l, r = l+1, r-1
    
    # or conditional update
    if ...:
        l +=1
    elif ...:
        r -=1
    else:
        ...

```

## Greedy

- Two Sum II (167)
- Largest container (11)

```python
# Task: Find largest container, contains most water
def maxArea(self, height: List[int]) -> int:
    l, r = 0, len(height) - 1

    maxArea = 0
    while l < r:
        maxArea = max(maxArea, min(height[l], height[r]) * (r - l))

        if height[l] < height[r]:
            l += 1
        else:
            r -= 1

    return maxArea
```


## Palindrome (125)
Task: judge whether it is a valid Palindrome.

`TENET`

## Reverse
- (345) Reverse vowels
- (917) Reverse letters

```python

def reverseVowels(self, s: str) -> str:
    vowels = set("aeiouAEIOU")
    s = list(s)

    l, r = 0, len(s)-1
    while l < r:
        if s[l] in vowels:
            if s[r] in vowels:
                s[l], s[r] = s[r], s[l]
                l += 1
                r -= 1
            else:
                r -= 1
        else:
            l+=1
    return "".join(s)

def reverseOnlyLetters(self, s: str) -> str:
    
    letters=set('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ')
    s = list(s)

    l, r = 0, len(s)-1
    while l < r:
        if s[l] in letters:
            if s[r] in letters:
                s[l], s[r] = s[r] , s[l]  # swap
                l+=1
                r-=1
            else:
                r-=1
        else:
            l+=1
    return ''.join(s)



```




