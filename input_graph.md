# Graph

graph and its given form

## Adjacency List (Most Common)

Representation: graph[i] is a list of nodes reachable from node i.

Typical Input Format: `graph = [[1,2], [3], [3], []]`
Meaning:
- Node 0 → {1, 2}
- Node 1 → {3}
- Node 2 → {3}
- Node 3 → {}

## Adjacency Map

Representation: graph[i] is a list of nodes reachable from node i.

Typical Input Format: `graph = {0: [1,2], 2: [3]}`
Meaning:
- Node 0 → {1, 2}
- Node 2 → {3}


## Adjacency matrix

Representation: matrix[i][j] = 1 if there is an edge from i to j, otherwise 0.

Typical Input Format:

```python
graph = [
  [0, 1, 1, 0],
  [0, 0, 0, 1],
  [0, 0, 0, 1],
  [0, 0, 0, 0]
]

```


## Edge list

Representation: A list of (from, to, weight) tuples.

Typical Input Format:

```python
edges = [(0, 1, 5), (0, 2, 3), (1, 3, 2), (2, 3, 4)]
```

## Parent list

Representation: Each index represents a node, and parent[i] is its parent.

Typical Input Format: `parent = [-1, 0, 0, 1, 1, 2, 2]`

Meaning:
- Node 0 is the root (-1).
- Nodes 1 and 2 are children of 0.
- Nodes 3 and 4 are children of 1, and so on.
