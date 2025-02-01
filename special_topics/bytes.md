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