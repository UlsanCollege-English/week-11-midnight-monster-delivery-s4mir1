[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/ulyILqqB)
# Weekly Coding #9: Midnight Monster Delivery

## Summary

This program finds the cheapest delivery routes through a haunted city using Dijkstra's algorithm. Each location in the city is represented as a node in a weighted directed graph, and each road between locations has a positive integer travel cost. The program can compute the minimum cost from a starting location to every other location, or reconstruct the exact shortest path between two specific locations. A validation function ensures the graph is well-formed before any algorithm runs.

## Approach

- **Graph representation:** The graph is stored as an adjacency dictionary (`dict[str, dict[str, int]]`), where each key is a node and its value is a dictionary of neighbors mapped to edge weights.
- **Priority queue/frontier:** A min-heap (`heapq`) stores `(cost, node)` tuples. The node with the lowest known cost is always processed first by popping from the heap.
- **Relaxation:** For each popped node, we check all its neighbors. If the new cost (current cost + edge weight) is less than the neighbor's known cost, we update the cost and push the neighbor onto the heap. Stale heap entries are skipped using a `current_cost > costs[node]` guard.
- **Path reconstruction:** `shortest_monster_delivery` maintains a `previous` dictionary that records which node we arrived from when relaxing each edge. After Dijkstra finishes, we walk backwards from the target to the start using `previous`, then reverse the list to get the start-to-target path.

## Complexity

```text
Time complexity: O((V + E) log V), where V is the number of locations and E is the number of roads.

Space complexity: O(V) extra space for distances, previous nodes, and the frontier. If we include graph storage, the total is O(V + E).
```

- `monster_delivery_costs`:
  - Time: O((V + E) log V)
  - Space: O(V + E) including the graph; O(V) extra for the `costs` dict and heap
  - Why: Every node is pushed to the heap at most once per incoming edge (E pushes total), and each heap operation costs O(log V). The `costs` dict and heap each hold at most V entries.

- `shortest_monster_delivery`:
  - Time: O((V + E) log V)
  - Space: O(V + E) including the graph; O(V) extra for `costs`, `previous`, and the heap
  - Why: Same Dijkstra traversal as above, with an additional O(V) `previous` dictionary for path reconstruction. Reconstructing the path itself is O(V) since the path visits each node at most once.

## Edge-Case Checklist

Check the cases your code handles.

- [x] start equals target
- [x] target is unreachable
- [x] start node is missing
- [x] target node is missing
- [x] node has no outgoing edges
- [x] graph contains cycles
- [x] tied shortest paths
- [x] negative edge weight
- [x] zero edge weight
- [x] neighbor not listed as a graph node

## Tests I Added

List any tests you added beyond the starter tests.

- No additional tests were added; all 14 provided tests pass with the current implementation.

## Assistance & Sources

AI used? Y

If yes, what did it help with?

- Helped implement `validate_haunted_map`, `monster_delivery_costs`, `shortest_monster_delivery`, and `best_next_monster_stop` using Dijkstra's algorithm with `heapq`.
- Verified the implementation against all test cases.

Other sources used:

- Python `heapq` documentation: https://docs.python.org/3/library/heapq.html
- Dijkstra's algorithm (Wikipedia): https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm

## Notes for Instructor

- The `best_next_monster_stop` stretch challenge is also implemented. It reuses `monster_delivery_costs` and returns the first reachable target with the lowest cost, preserving tie-breaking order from the input list.
- All 14 public tests pass.