# Exp-No 4 : Implement A* search algorithm for a Graph
### Name: Sanjana Sri N
### Register Number: 2305003007
## Aim:
To Implement A * Search algorithm for a Graph using Python 3.

## Algorithm:

### A* Search Algorithm

 1. Initialize

       - Open list ← priority queue (min-heap)

       - Closed list ← empty set

       - Insert the start node into the open list with:

           - g = 0

           - h = heuristic(start)

           - f = g + h

2. While Open list is not empty:
   
      - Remove the node q with the lowest f from the open list.
   
      - If q is the goal node, return the path (reconstruct from parents).
   
      - If q is already in the Closed list, skip it.
   
      - Add q to the Closed list.

3. For each neighbor n of q:

      - Compute:

           - g(n) = g(q) + cost(q, n)

           - h(n) = heuristic(n)

           - f(n) = g(n) + h(n)

     - If n is not in Closed list:

       - Insert (f(n), g(n), n, path_so_far) into Open list.

4. If goal is never reached and Open list becomes empty:

    - Return "No path found".


## SAMPLE PROGRAM:

```python

from collections import defaultdict
H_dist ={}
def aStarAlgo(start_node, stop_node):
    open_set = set(start_node)
    closed_set = set()
    g = {}  
    parents = {}   
    g[start_node] = 0
    parents[start_node] = start_node
    while len(open_set) > 0:
        n = None
        for v in open_set:
            if n == None or g[v] + heuristic(v) < g[n] + heuristic(n):
                n = v
        if n == stop_node or Graph_nodes[n] == None:
            pass
        else:
            for (m, weight) in get_neighbors(n):
                if m not in open_set and m not in closed_set:
                    open_set.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight
                else:
                    if g[m] > g[n] + weight:
                        g[m] = g[n] + weight
                        parents[m] = n
                        if m in closed_set:
                            closed_set.remove(m)
                            open_set.add(m)
        if n == None:
            print("Path does not exist!")
            return None
        if n == stop_node:
            path = []
            while parents[n] != n:
                path.append(n)
                n = parents[n]
            path.append(start_node)
            path.reverse()
            print('Path found: {}'.format(path))
            return path
        open_set.remove(n)
        closed_set.add(n)
    print('Path does not exist!')
    return None
def get_neighbors(v):
    if v in Graph_nodes:
        return Graph_nodes[v]
    else:
        return None
def heuristic(n):
    return H_dist[n]
graph = defaultdict(list)
n,e = map(int,input().split())
for i in range(e):
    u,v,cost = map(str,input().split())
    t=(v,int(cost))
    graph[u].append(t)
    t1=(u,int(cost))
    graph[v].append(t1)
for i in range(n):
    node,h=map(str,input().split())
    H_dist[node]=int(h)

Graph_nodes=graph
start=input()
goal=input()

aStarAlgo(start, goal)
```

### SAMPLE GRAPH I:
![277151990-b1377c3f-011a-4c0f-a843-516842ae056a](https://github.com/user-attachments/assets/bedfaca4-a69a-468a-957d-a3def3f28836)

### SAMPLE INPUT I:

```
10 14 
A B 6 
A F 3 
B D 2 
B C 3 
C D 1 
C E 5 
D E 8 
E I 5 
E J 5 
F G 1 
G I 3 
I J 3 
F H 7 
I H 2 
A 10 
B 8 
C 5 
D 7 
E 3 
F 6 
G 5 
H 3 
I 1
J 0 

```
### SAMPLE OUTPUT I: 

![435098048-ac8a5725-93b0-44e9-bba7-8a32d7ee80ee](https://github.com/user-attachments/assets/23a66732-b6f5-4b74-8eae-e3e21b0f6814)

### SAMPLE GRAPH II: 

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)

### SAMPLE INPUT II: 
```
6 6
A B 2
B C 1 
A E 3 
B G 9 
E D 6 
D G 1 
A 11 
B 6 
C 99 
E 7 
D 1 
G 0 
```
### SAMPLE OUTPUT II:

![435098114-175123a9-8519-4ec4-b3f6-1151b674380d](https://github.com/user-attachments/assets/91f98b2e-c195-45de-9dbf-19bc17dbfb8f)

## PROGRAM: 

```python

from collections import defaultdict

H_dist = {}
def aStarAlgo(start, goal):
    open_set = {start}
    closed_set = set()
    g = {start: 0}
    parent = {start: start}
    while open_set:
        n = min(open_set, key=lambda x: g[x] + heuristic(x))
        if n == goal:
            path = []
            while parent[n] != n:
                path.append(n)
                n = parent[n]
            path.append(start)
            path.reverse()
            print("Path found:", path)
            return path

         for neighbor, cost in get_neighbors(n):
            new_cost = g[n] + cost
            if neighbor not in open_set and neighbor not in closed_set:
                open_set.add(neighbor)
                parent[neighbor] = n
                g[neighbor] = new_cost
            elif new_cost < g.get(neighbor, float('inf')):
                g[neighbor] = new_cost
                parent[neighbor] = n
                if neighbor in closed_set:
                    closed_set.remove(neighbor)
                    open_set.add(neighbor)
        open_set.remove(n)
        closed_set.add(n)
    print("Path does not exist!")
    return None

def get_neighbors(node):
    return Graph_nodes.get(node, [])

def heuristic(node):
    return H_dist.get(node, 0)

graph = defaultdict(list)
n, e = map(int, input("Enter number of nodes and edges: ").split())

for _ in range(e):
    u, v, cost = input().split()
    cost = int(cost)
    graph[u].append((v, cost))
    graph[v].append((u, cost))  # undirected

for _ in range(n):
    node, h = input("Enter node and heuristic: ").split()
    H_dist[node] = int(h)

Graph_nodes = graph

start = input("Enter start node: ")
goal = input("Enter goal node: ")

aStarAlgo(start, goal)

```

## INPUT GRAPH:
![WhatsApp Image 2025-10-08 at 11 41 54_d9de9439](https://github.com/user-attachments/assets/c4915d24-8ae5-4a19-814a-a2d8bcb01878)

## INPUT: 
<img width="406" height="366" alt="image" src="https://github.com/user-attachments/assets/83b4b54b-ec7b-4759-be0d-f2cbe1a3f5ed" />

## OUTPUT: 
<img width="378" height="73" alt="image" src="https://github.com/user-attachments/assets/fc33040b-03ab-4f4b-b071-0595d44dafe6" />

## RESULT: 
Thus, the A* algorithm has been successfully implemented and executed to find the optimal path between two nodes in a given weighted graph.
