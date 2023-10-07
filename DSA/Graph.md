## Time Complexity

`V` is the number of vertices. `E` is the number of edges

| Algorithm            | Big-O      |
| -------------------- | ---------- |
| Depth-first search   | O( V + E ) |
| Breadth-first search | O( V + E ) |
| Topological sort     | O( V + E ) |

## Things to Consider

- A tree-like diagram could also be a graph that allows for cycles, and a naive solution won't work. In that case you will have to handle cycles and keep a set of visited nodes when traversing.

- Empty graph
- Graph with one or two nodes
- Disconnected graphs
- Graph with cycles

## Graph Search Algorithms

- Common - Breadth-first, Depth-first
- Uncommon - Topological Sort, Dijkstra's algorithm
- Almost never - Bellman-Ford algo, Floyd-Warshal algo, Prim's algo, Kruskal's algo
