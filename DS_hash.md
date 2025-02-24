# HashSet & HashMap
Use set/dict to solve problem.
(because set/dict is a hash map and is fast to trace)

Usually, 
- we use set to:
  - record **Visited**.
  - quickly find sequential **Existance** with while loop.
- we use dict:
  - as counter to count **Freq** or **Degree** in graph.
  - as locator (recording **Index** and associatives), to track things we focus (eg, char) or target/mid-target.

## Set

### Visited
Use a set to record visited points.

#### Linked-List Cycle(141) and Happy number(202)

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

### Exists

#### Longest Consecutive (128)

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


## Map

### Counter
Use Map-Counter to solve the problem

- 350 Array Intersection II : count frequency
- 76 Including sub-array: count frequency
- 997 Town judge: count degree

#### Counter for middle result
`(mid-target: count)`:
- (count) 560 k-sum sub-array

#### Counter as middle result

- (#pairs) 447 Boomerangs amount
- (#pairs) 1128 Domino pairs
- (#pairs) perfect pairs



#### Classic: Anagram
specific case: Freq counter of chars in a string.

**Valid Anagram(242)**

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

**Group Anagram(49)**

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

Alternatively, you can also use `tuple(sorted(counter(s)))` as key if characters are not limited to small letter english.

**All Anagram(438)**

Task: return start index of all anagrams of a certain string within a  longer string, given that certain string.

#### Special
- 791 Custom sort (solution: use counter)

### Locator
`(target: index)`:
- (index) 1 Two Sum
  
`(mid-target: index)`:
- (index) 525 balance sub-array

`(char: index)`:
- (index) 3 Longest Non-repeating
- **(index & ...) 697 Array degree**

**Array degree**

Task: find (the length of) a minimum sub-array with the same degree of the original array.
(def. Array degree: max(counter.values()))

Solution: Use a hash map to store `(char: (degree, start, end))`

```python

def findShortestSubArray(self, nums: List[int]) -> int:

    # specific hashmap
    store = {}
    for i in range(len(nums)):
        if nums[i] not in store:
            store[nums[i]] = [1,i,i] # (frequency, startIdx, endIdx)
        else:
            store[nums[i]][0] +=1
            store[nums[i]][2] = i
    
    # use this hashmap to easy get result
    maxFreq = max(val[0]  for val in store.values())
    minlen = float("inf")
    for freq, start, end in store.values():
        if freq == maxFreq:
            minlen = min(minlen, end-start+1)
    
    return minlen

```
### Classic: sub-arrays
- 3 non-repeating sub-array
- 697 same-degree sub-array
- 525 balance sub-array
- 560 k-sum sub-array
- 76 including sub-array

(see more sub-array related challenges in `special_topics/sub_array.md`)

when a search of the previous index is needed.
