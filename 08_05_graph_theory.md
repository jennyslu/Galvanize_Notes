# Morning Lecture

__Objectives__

1. What is a graph?
2. What does it represent?
3. What kinds of graphs are there?
4. How do we store graphs as data?
5. How do we traverse graphs and why?
6. How does Breadth First Search work?

## Undirected weighted graphs

 - FB friends with weight being number of messages between
 - Postal network with weight being cost of postage between
 - Gravity between planets
 - At networking event where weight is social cost to talking to someone (low if friends, high if fancy stranger)

## Terminology

Graphs have _vertices_ and _edges_

__Neighbours__ are directly connected to N

__Degree__ of N is number of neighbours N has

__Path__ set of unique nodes and edges that connect N to M

__Subgraph__ any subset of nodes and their edges

__Connected component__ a subgraph that is connected to itself (not to another connected component)

__Complete graph__ all nodes are connected

__Simple graph__ no edges connecting node to itself and also no more than one edge directly connecting two nodes

## Representation

```python

## UNDIRECTED

edges = [(A,B), (A,C), (B,C), (B,D)]

# order (V + E)
# how much space it will take up
adjacency_list = {A: [B,C ],
                    B: [A, C,D],
                    C: [A, B],
                    D: [B]}

# order (V^2)
# will always be symmetric
adjacency_matrix = [[0, 1, 1, 0],
                    [1, 0, 1, 1],
                    [1, 1, 0, 0],
                    [0, 1, 0, 0]]

# weighted
# cannot put 0 where there is no edge
# computer will take this to mean that there IS an edge with weight 0
# when trying to find path it will pick AD, which doesn't exist
# put big number instead (sum of all weights) + 1000 (can't really use inf)
adjacency_matrix = [[0, 3, 5, inf],
                    [1, 0, 1, 1],
                    [1, 1, 0, inf],
                    [0, inf, inf, 0]]

## DIRECTED

# order (V^2)
# will not be symmetric
adjacency_matrix = [[0, 1, 0, 0, 0, 0],
                    [0, 0, 1, 1],
                    [1, 1, 0, 0],
                    [0, 1, 0, 0]]

```

## Traversing

### Breadth First Search

Given a start and end node: _the first path found by BFS is guaranteed to be the shortest path_. __For unweighted graphs only__. Dijstra works with non-negative weighted graphs.

However, _memory intensive_

1. Start at node
2. Look at all neighbours first. Put in list Q
3. Remove each neighbour from Q on FIFO basis
 - add it to set of visited nodes V
 - add all its neighbors to end of list Q

#### Real pseudocode

Shortest from A to C?

1. Create empty queue Q (current stage of nodes to check - BREADTH FIRST)
2. Create empty set V (visited nodes)
3. Add the tuple (A,0) to Q
4. While Q is not empty:
 - Remove first element from Q. It will be tuple (N,d) representing node N with a distance d from A
 - if N is the desired node:
   * RETURN (N,d)
 - if N is not in V:
   * add N to V
   * add every neighbour of N to the end of Q with distance d+1

### Depth First Search

1. Pick starting node
2. Pick neighbour node, add it to your set of visited nodes
3. Pick a niehgbour node of that neighbour node, add it to V
4. Repeat until you find no neighbours that you haven't visited, then backtrack until you find a node with a fresh enighbour
5. Repeat until you have visited every node

__Last in first out queue__ vs BFS __first in first out queue__. __Dijstra is first out based on smallest weight__.

# Afternoon Lecture

__Objectives__

1. Makes a node important
 - centrality measures
   * degree
   * B/W
   * eigenvector

2. What are communities?

3. How do we find them?
 - modularity
 - hierarchical

## How important is a given node?

### Degree centrality

Number of links node has

### Betweenness centrality

Number of pairs that pass through this node as part of shortest path between the pair

### Importance?

What does it mean if a node has high degree centrality but low between centrality?

### Eigenvector centrality

Important nodes are connected to other important nodes. This is the whole idea behind eigenvector centrality.

E.g. LinkedIn endorsements. Do we want to hire someone who has two endorsements by awesome people or someone who has 100 endorsements from unimportant people?

__Adjacency matrix = A__

__Degree centrality vector = x__

Since some nodes are connected to unimportant nodes (i.e. nodes that have low degree centrality). By multiplying A with x we can account for this.

Now that we have discounted node 4's importance because node 5 is less important.

But now we also want to discount all the nodes connected to node 4 because it is less important than we previously thought.

Let's multiply adjacency matrix with result of previous to account for this.

We can do this forever. It will keep getting bigger and bigger. How can we fix this? Normalize by dividing result by norm of result.

Thus eigenvector centrality was born!

## Communities

__Mutual ties__: within the group know each other

__Compactness__: reach other members in few steps

__Dense edges__: high frequency of edges within groups

__Separation from other groups__: frequency within groups higher than out of group

### Modularity

__Only a heuristic__

Observed fraction of links within groups - expected fraction of links in the group

Each stub is an opportunity to connect.

This is too many partitions to do even with just 20 nodes.

_Let's look at edges instead_

__This is the opposite of hierarchical clustering__. Can we just do hierarchical clustering then?

# Afternoon Breakout

When we calculate modularity we use the original graph. Not cut into clusters one.
