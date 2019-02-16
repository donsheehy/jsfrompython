# Data Structures

One way to really get a good feel for a language is to code up some more complex data structures.

## A Graph Implementation

Perhaps the easiest way to efficiently represent a directed graph in Python is to store the edges as a dictionary mapping start vertices to a set of end vertices.

```python {cmd}
class Graph:
    def __init__(self, V = (), E = ()):
        self._nbrs = {}
        for v in V:
            self._nbrs[v] = set()
        for u,v in E:
            self._nbrs[u].add(v)

    def nbrs(self, v):
        return iter(self._nbrs[v])

    def __len__(self):
        return len(self._nbrs)


V = {1,2,3,4,5}
E = {(1,2), (2,1), (3,4), (4,5), (2,5)}
G = Graph(V, E)

print(list(G.nbrs(2)))
```
