# Transform
Keep the data-structure, change in-place or not. Or, alter the data-structure, transfrom the inputs into wanted format. 

## Self-transform

### in-place
- array/string
    - 345	Reverse vowels
    - 917	Reverse letters
    - 26	Remove duplicates (`aab`->`ab`)
    - 443	Compress duplicates (`aab`->`a2b`)
    - 88	In-place mergeSort
- matrix (board)
    - 73	boob in matrix
    - 733	Flood Fill
    - 130   Surrounded region

### same type and shape

Sort: 
- 23 MergeSort
- 791 Custom sort
- 451 Freq-based sort
- 767 Distinctive adjacent

### same type
- (66) Plus One
- (56) Merge intervals
- (57) Insert interval
- (435) Non-overlapping Intervals (hint: sort by ends)


## Build

- 218 skyline
- 31 Next permutation

### Classic

#### Routing
Find one path or collect all valid paths in a tree or graph

- routing (Tree/Graph -> Array)
  - 209 Course Schedule
- routes collection (Tree/Graph -> Array(Array/ListNode))
  - 113 Path Sum II
  - 797 All Source2Target
  
#### Partition
- string partition (str <-> Array(string))
  - 131 Palindrome partition
  - 763 non-crossing partition (output as substrings)
- array partition/grouping (Array(int/char) -> Array(Array))
  - 49 Group anagrams

#### Collection
Collect all valid combinations. Mostly using dfs-backtracking. 

- Collection (Array(int/char) -> Array(Array))
  - 46 Permutation
  - 78 Subsets
  - 90 Subsets II (hint: first sort, when skip, skip all the rest)


### General

#### int <-> str
- 12 Int2Roman
- 13 Roman2Int

#### int <-> Array(int)
- 118,119 Pascal Triangle

#### Array(int) <-> Array(int/char/str)
->:
- 506 Rank & Medal
- 17 Phone typing

<-:
- 821 closest occurrence
- 542 (2D) closest zero

#### Tree <-> Array/ListNode
- 114 Flatten BST
- 108 Sorted2BST

