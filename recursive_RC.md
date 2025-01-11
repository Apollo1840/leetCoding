# Reduce and Conquer
- A special form of DC, where the problem size is divided as n-1 and 1.
- A special form of DP, where intermediate results is not explicitly stored because there is no reuse.

challenges:
- 66 plus one

## Pascal triangle(118)
Task: generate Pascal triangle of given height:

```python

 def generate(self, numRows: int) -> List[List[int]]:
        """
        Recursive: DC-1

        """

        # start point
        if numRows == 0:
            return []
        if numRows == 1:
            return [[1]]
        
        # get previous result
        res_prev = self.generate(numRows - 1)

        # use previous result to generate current result
        new_row = [res_prev[-1][i] + res_prev[-1][i+1] for i in range(len(res_prev[-1])-1)]
        new_row = [1] + new_row +[1]

        res = res_prev + [new_row]

        return res

```

- 119 Pascal triangle II



