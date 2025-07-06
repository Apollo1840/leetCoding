# HashSet & HashMap
Use set/dict to solve problem.
(because set/dict is a hash map and is fast to trace)

Usually, 
- we use set to:
  - record **Visited**(141, 202, 36).
  - quickly find sequential **Existance**(128) with while loop.
- we use dict:
  - as counter to count **Freq**(350, 76, 791) or **Degree**(997) in graph.
  - as locator (recording **Index** and associatives), 
    to track things we focus (eg, char) or target/mid-target. (1, 3, 525)

## Set
Key idea: It is very fast to look up. 


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
    # solution: locate the start of each consecutive sub array, 
    # then count how long is it. 
    
    num_set = set(nums)
    res = 0

    for n in num_set:
        if n-1 in num_set:
            continue
        
        length = 1
        while n + length in num_set:
            length += 1
        res = max(res, length)

    return res

```


## Map

### Counter
Counter is a Map(dict) to count something(mostly freq/degree) of elements.

For freq of the items:

- 350 Array Intersection II
  (Find common elements in two arrays including duplicates)
- 76 Including sub-array
  (Find the minimum window in one array covering another sub-array)
- 791 Custom sort

For degree of the vertices:

- 997 Town judge
  (Identify the one person trusted by everyone but who trusts no one)


#### Classic: Anagram
A special case of Counter, where we specifically use Map-Counter to get the freq of the items in a string.

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

Task: give a string, return start index of all anagrams of that certain string within a  longer string.

Hint: a running fixed size screen.

```python

  def findAnagrams(self, s: str, p: str) -> List[int]:
      # edge case
      if len(s) < len(p):
          return []

      target_counter = Counter(p)
      l = len(p)
      n_matched_target = len(target_counter)
      n_matched = 0
      res = []

      for i in range(len(s)):

          # move forward s[i]
          if s[i] in target_counter:
              target_counter[s[i]] -= 1
              if target_counter[s[i]] == 0:
                  n_matched += 1

          # move forward s[i-l]
          if i >= l and s[i - l] in target_counter:
              target_counter[s[i - l]] += 1
              if target_counter[s[i - l]] == 1:
                  n_matched -= 1
              
          # check match only when window size == len(p)
          if n_matched == n_matched_target:
              res.append(i - l + 1)

      return res

```

This challenge is strongly related to: 76 (shortest) Including sub-array.

#### Count middle result
`(mid-target: count)`:
- 560 k-sum sub-array 
  (Count subarrays whose elements sum to k)
  (hint: use `pre_sum: count`)

#### Counter as middle step

- (#pairs) 447 Boomerangs amount
- (#pairs) 1128 Domino pairs
- (#pairs) perfect pairs


### Locator
Locator is a Map(dict) record indices of key elements.

`(char: index)`:
- (index) 3 Longest Non-repeating
- **(index & ...) 697 Array degree**

`(target: index)`:
- (index) 1 Two Sum
  
`(mid-target: index)`:
- (index) 525 balance sub-array

**Longest non-repeating**

```python

  def lengthOfLongestSubstring(self, s: str) -> int:
      locator = dict()  # (char: index)
      res = 0
      start_index = -1
      for i, char in enumerate(s):
          
          if char in locator:
              start_index = max(start_index, locator[char])

          # update locator
          locator[char] = i

          # catching result
          res = max(res, i- start_index)

      return res

```



**525 balance sub-array**

```python

def findMaxLength(self, nums):
    pre_sum_locator = dict()  # Store {pre_sum: first_index}
    pre_sum_locator[0] = -1
    
    res = 0
    pre_sum = 0

    for i, num in enumerate(nums):
        pre_sum += num if num == 1 else -1

        if pre_sum in pre_sum_locator:
            res = max(res, i - pre_sum_locator[pre_sum])
        else:
            pre_sum_locator[pre_sum] = i  # Store first occurrence

    return res
            

```

This challenge is strongly related to:  560 k-sum sub-array.

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
- 3 (longest) non-repeating sub-array
- 525 (longest) balance sub-array
- 76 (shortest) including sub-array
- 697 same-degree sub-array
- 560 k-sum sub-arrays

(see more sub-array related challenges in `special_topics/sub_array.md`)

when a search of the previous index is needed.
