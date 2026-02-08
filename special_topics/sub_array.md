# Sub-array/Substring

## Definition
A **Sub-array** is a **contiguous** portion of an array.

Meaning it consists of elements that appear sequentially in the original array and maintains their original order.

so it also can be interpreted as finding two indices.

If the array is an array of chars, it is also called **Substring**.

(p.s A **Sub-sequence** is a **non-contiguous** but same order portion of an array.)

## Understanding

Since we are dealing with two indices, 
it is common that we use:
-  **Locator** (3, 525, 697)
- and **LeftRight Pointers** (209, 76) 

to solve such problem.

In some case, it also use **2-D DP** (5, 718).

## Challenges

### prefix
- 14 common prefix

### single sub-array

- 3 (longest) non-repeating sub-array
- 525 (longest) balance sub-array
- 5 (longest) palindrome sub-array

```python
# 3 (longest) non-repeating sub-array
def lengthOfLongestSubstring(self, s: str) -> int:
      locator = dict()  # (char: previous_index)
      res = 0
      start_index = -1
      for i, char in enumerate(s):
          
          if char in locator:
              start_index = max(start_index, locator[char])
    
          # update locator
          locator[char] = i
    
          # catching result
          res = max(res,  i - start_index)
    
      return res

```

- 209 (shortest) large sub-array
- 76 (shortest) Including sub-array
- 697 (shortest) same-degree sub-array

  

```python
# 76 (shortest) Including sub-array
def minWindow(self, s: str, t: str) -> str:
    target_counter = Counter(t)
    res = ""
    p1, p2 = 0, 0

    n_matched = 0
    n_matched_target = len(target_counter)

    while p2 < len(s):
        # 先扩展右边窗口
        if s[p2] in target_counter:
            target_counter[s[p2]] -= 1
            if target_counter[s[p2]] == 0:
                n_matched += 1
        p2 += 1 

        # 如果满足所有字符要求，尝试收缩窗口
        while n_matched == n_matched_target:
            # 更新结果
            if len(res) == 0 or p2 - p1 < len(res):
                res = s[p1:p2]

            if s[p1] in target_counter:
                target_counter[s[p1]] += 1
                if target_counter[s[p1]] == 1:
                    n_matched -= 1
            p1 += 1

    return res

```

- 53 Max sub-array
- 152 Max-prod sub-array



### sub-array relation
- 718 (longest) common sub-array


### collect sub-arrays
- 560 k-sum sub-arrays
- 763 non-crossing sub-arrays
- 438 anagram sub-arrays

