# How does Google Maps work ?

A **Web mapping** or an online mapping is the process of using the maps delivered by geographic information system on the Internet. With the rise of web mapping, software which provide directions from one place to another have come into existence, such as Google Maps.

The algorithm behind the fastest route comes is Dijkstra’s algorithm. We will now explore this algorithm.

## Dijkstra’s algorithm

We first model our map as a weighted graph, where each turn is node and the roads between them as edges. The weight of the edge depends on various conditions such as length, traffic and other conditions.
This data is updated in real time by Google using the data collected from other users using the software. 

![img](https://magazine.impactscool.com/wp-content/uploads/2019/05/grafi-e-nodi-1024x308.png)

Lets assume weight of an edge is directly proportional to the time taken taken to move along that edge/road. In terms of this graph we will now restate our original problem as

> Given a directed graph with unbounded non-negative weights, find the path from source to destination such that the sum of weights of all edges in the path is minimized.

The algorithm tracks a cost associated with each node, that is the shortest time taken to reach a particular node. The algorithm is as follows

1. Initialize the cost of all nodes to infinity, except the starting node, which is set to 0.
2. Initialize a list of nodes to be visited.
3. For the current node, consider all of its unvisited neighbors and calculate their cost from the current node. That is cost of neighbor node is equal to the sum of cost of current node and weight of the edge connecting them. If the value obtained is lower than the cost of neighbor node, it is updated, otherwise its left unchanged.
4. Once all the neighbors of the current node are visited, it is removed from the list of nodes to be visited.
5. If the destination node has been marked visited or if the weight of all nodes in the list of nodes to be visited is infinity, there is no path from source to destination, then the algorithm has completed.
6. Otherwise, select the node from the list of nodes to be visited with the least cost, set it as the current node and repeat from step 3.

The pseudocode for this algorithm is given by 



```python
function Dijkstra(Graph, source):
      create vertex set Q
 
       for each vertex v in Graph:            
           dist[v] ← INFINITY                 
           prev[v] ← UNDEFINED                
           add v to Q                     
       dist[source] ← 0                       
     
       while Q is not empty:
           u ← vertex in Q with min dist[u]   
                                              
           remove u from Q
          
           for each neighbor v of u still in Q:
               alt ← dist[u] + length(u, v)
               if alt < dist[v]:              
                   dist[v] ← alt
                   prev[v] ← u
 
       return dist[], prev[]
```

Another faster algorithm for the same purpose is the **A* algorithm**

## A* Algorithm

**A* Algorithm** is an informed search algorithm, or a best-first search. Informed search algorithm use additional information that directs the search towards the goal. This additional information is usually just an approximation.

Starting from source, the algorithm aims to find a path to the given goal node having the smallest cost. It does this by maintaining a tree of paths originating from the start node and extending those paths one edge at a time until its termination criterion is satisfied. In each iteration the algorithm determines which of its paths to extend. The algorithm selects the path that minimizes $f(n) = g(n) + h(n)$, where n is the next node, $g(n)$ is cost of the path and $h(n)$ is a heuristic function.

A few examples of heuristic functions are:

1. Manhattan Distance
   It is the sum of absolute values of differences in the destination’s x and y coordinates and current cell’s x and y coordinates. $h(x) = abs(x_{dest}-x_n)+abx(y_{dest}-y_n)$
2. Euclidean Distance
   It is the distance between the current node and the destination in the 2D plane. $h(x) = \sqrt{(x_{dest}-x_n)^2+(y_{dest}-y_n)^2}$

If the heuristic $h(n)$ satisfies the additional condition that $h(x) \leq d(x,y)+h(y)$ for every edge $(x,y)$ of graph, then h is called monotone or consistent. With a consistent heuristic, A* is guaranteed to find an optimal path without processing any node more than once and A* is equivalent to running Dijkstra’s algorithm with $d'(x,y) = d(x,y)+h(y)-h(x)$. The pseudocode for this algorithm is given by

```pseudocode
// Initialize both open and closed list
let the openList equal empty list of nodes
let the closedList equal empty list of nodes

// Add the start node
put the startNode on the openList (leave it's f at zero)

// Loop until you find the end
while the openList is not empty
    // Get the current node
    let the currentNode equal the node with the least f value
    
    remove the currentNode from the openList
    add the currentNode to the closedList
    // Found the goal
    if currentNode is the goal
        Congratz! You've found the end! Backtrack to get path
    
    // Generate children
    let the children of the currentNode equal the adjacent nodes
    
    for each child in the children
        // Child is on the closedList
        if child is in the closedList
            continue to beginning of for loop
        // Create the f, g, and h values
        child.g = currentNode.g + distance between child and current
        child.h = distance from child to end
        child.f = child.g + child.h
        // Child is already in openList
        if child.position is in the openList's nodes positions
            if the child.g is higher than the openList node's g
                continue to beginning of for loop
        // Add the child to the openList
        add the child to the openList
```

You can compare both the algorithms by looking at the visualizations links given below

- [Dijkstra’s Algorithm](https://upload.wikimedia.org/wikipedia/commons/2/23/Dijkstras_progress_animation.gif)
- [A* Algorithm](https://upload.wikimedia.org/wikipedia/commons/5/5d/Astar_progress_animation.gif)
