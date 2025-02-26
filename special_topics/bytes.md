# Bytes

```python

    # integer to bytes
    b_str = format(i, "b")
    b_str = format(i, "b").zfill(32)  # bytes in 32 bits
    
    # bytes to integer
    i = int(b_str, 2)
    
    # XOR
    i ^ j
    
    # ones count
    i.bit_count()
    
    # take last bit
    j = i & 1
    
    # remove last bit
    i = i >> 1
    
    # create bit
    1 << n  # (= 2**n )

```

- 190 Reverse Bits
- 693 non-repeating bits
- 2220 minimum flips

## Bit Trick



### XOR-cancel out
- `a ^ 0 = a`
- `a ^ a = 0`
- `a ^ b = b ^ a` (commutative)
- `a ^ b ^ c = a ^ (b ^ c)` (associative)

=>

`a ^ b ^ b ^ A ^ a = A`

  
examples:
- 136 Single number
- 268 Missing number
- 389 Find difference
- 1310 XOR sections

### The Last one
- `a & (-a)`: last 1-bit remain. 
  
examples:
- (231) power of two
- (260) Single number II

### Bit mask
- `(1 << n) - 1` all 1's of length (n-1)
- `1 << i` only position i as 1.
- `mask | (1 << i)` mark i as 1.

- 847 Shortest traversal
