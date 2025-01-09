# HashMap

## Anagram

Use Map-Counter to solve the problem

### Valid Anagram
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

### Group Anagram
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
