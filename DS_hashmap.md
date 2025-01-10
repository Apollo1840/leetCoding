# HashMap
Use set/dict to solve problem.
(because set/dict is a hash map and is fast to trace)

Usually, 
- we use set to record **Visited** (141, 202), 
or we use set to quickly find sequential **Existance** (36) with while loop.
- we use dict as counter to count **Freq** (242, 49, 350) or **Degree** (997).
- a more tricky use of dict is to record **Index** (1, 3, 679) and associatives.

## Set-Visited
Use a set to record visited points.

### Linked-List Cycle(141) and Happy number(202)

```python

def hasCycle(self, head: Optional[ListNode]) -> bool:
    visited = set()

    while head:
        if head in visited:
            return True
        else:
            visited.add(head)
            head = head.next
    return False


def isHappy(self, n: int) -> bool:
    """
    It is not happy number if it hits visited number after calculation.
    so it is very similar to 141. Linked List Cycle
    """
    
    visited = set()
    
    while n != 1:
        if n in visited: 
            return False
        else:
            visited.add(n)
            n = sum([int(i) ** 2 for i in str(n)])
    return True



```

More visited trackers: Valid sudoku (36)

## Set-Exists

### Longest Consecutive (128)

Task: count the length of the largest consecutive sub array exists in a given array.

```python

def longestConsecutive(self, nums: List[int]) -> int:
    num_set = set(nums)
    longest = 0

    for n in num_set:
        if n-1 in num_set:
            continue
        
        length = 1
        while n + length in num_set:
            length += 1
        longest = max(longest, length)

    return longest

```


## Counter


### Array Intersection II (350)
count frequency


### Town judge(997)
count degree



## Counter - Anagram

Use Map-Counter to solve the problem

### Valid Anagram(242)
Task: judge whether a pair of strings is anagram or not.

```python

def isAnagram(self, s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    c = defaultdict(int)
    for i in range(len(s)):
        c[s[i]] += 1
        c[t[i]] -= 1

    return all(v == 0 for v in c.values())

```

### Group Anagram(49)
Task: return groups of anagrams.

Solution: use tuplized Map-counter as key.

```python

def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
    anagrams = defaultdict(list)
    
    for s in strs:
        # 用字符频次作为 key
        count = [0] * 26  # 只针对小写字母
        for char in s:
            count[ord(char) - ord('a')] += 1
        anagrams[tuple(count)].append(s)

    return list(anagrams.values())


```

you can use `tuple(sorted(counter))` as key if characters are not limited to small letter english.

## Map
- (index) 1 Two Sum
- (index) 3 Longest Non-repeating
- **(index & ...) 697 Array degree**

## Challenges
- 791 Custom sort