
## Map

- put(key, value)
- get(k)
- remove(k)

# Graphs

- vertex - a node in the graph
- edge - a pair of vertices representing a connection between two nodes in a graph
- undirected graph - a graph in which edges have no direction
- directed graph - a graph in which each edge goes from one vertex to another (or same) vertex
- adjacent vertices - two vertices in a graph that are connected by an edge
- path - a sequence of vertices that connects two nodes in a graph

Formally, a graph G = (V, E), where V(G) is a finite, nonempty set of vertices, and E(G) is a set of edges (written as pairs of vertices). 

- V(G1) = {A, B, C, D}
- E(G1) = {(A, B), (A, D), (B, C), (B, D)}

Learn more about graph in an algorithm/discrete math course. 

A tree is a connected graph without cycles. 

Directed Acyclic Graph - usually used to denote some order constraints. 

How do we represent a graph in C code? 

### Graph example

Consider the following flight map of CS152 Airline. 

- V: {LA, SF, CHI, Houston, NYC, DC}
- E: {(SF, CHI), (CHI, Houston), (Houston, DC), (Houston, NYC), (NYC, LA), (LA, NYC), (DC, SF)}

This is a directed graph. 

- Question: Does my airline fly from SF to NYC? 
- Does my airline fly from LA to DC? 

Given me the routes

## Depth First Search (DFS)

Pseudo code of DFS:
```
void DFS(graph* g, vertex* v):
    v->visited = true
    for all edges from v to w in G.adjacentEdges(v) do:
        if !w->visited then:
            DFS(w)
```

Initialization:
```
for all nodes v in G
    v->visited = false
```

Modify DFS to solve our problem:
```
procedure DFS(graph* g, vertex* v, vertex* dest):
    v->visited = true
    if v == dest:
        return true
    for all edges from v to w in g do:
        if !w->visited then
            w->prev = v
            if (DFS(G, w, dest))
                return true
    return false
```

Question 2: We want to buy an airline ticket from CHI to SF. Can I get a ticket with no more than one stopover? What routes are we going to search? 

Initialization:
```
for all nodes v in G
    v->visited = false
```

```
procedure DFS(graph* g, vertex* v, vertex* dest, int hops):
    if (hops >= 2)
        return false
    v->visited = true
    if (v == dest)
        print route from start to dest; return true;
    for all edges from v to w in g do:
        if !w->visited then:
            if DFS(G, w, dest, hops+1) == true:
                return true
    return false

```

## Breath First Search (BFS)

visit notes more akin to level order. Start with node v, then visit all nodes connected to v. Then connect all nodes connected to v's adjacent nodes, etc. 

```
int bfs(graph* g, vertex* v, vertex* dest, int parent[], int max_hops):
    queue* q = create_queue();
    queue_push(q, v);
    v->visited = true;
    v->prev = NULL;
    v->depth = 0;
    while (!queue_empty(q)){
        vertex v = queue_dequeue(q);
        if (v->depth > max_hops)
            return 0;
        else if (v == t)
            return 1;
        
        for each vertex w for which there is Edge(v, w){
            if ((!w->visited)){
                queue_push(w, q);
                w->prev = v;
                w->visited = true;
                w->depth = v->depth + 1;
            }
        }
    }
    return 0;
```

## Dijkstra's Algorithm


Consider a map planning problem. 

1. Assign to every node a tentative distance value: set it to zero for our initial node and to infinity for all other nodes. Mark all nodes unvisited. Create a set of all the unvisited nodes called the unvisited set.
2. If the smallest tentitive disntance among the nodes in the unvisited set is infinity, then stop. The algorithm has finished. 
3. Select the unvisited node that is marked with the smallest tentative distance, set it as "current node", mark it as visited. 
4. If the destination node has been marked as visited (when planning a route between two specific nodes), The algorithm has finished. 
5. For the current node, consider all of its unvisited neighbors and calculate their tentative distances. Compare the newly calculated tentative distance to the current assigned value and assign the smaller one. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbor B has length 2, then the distance to B (throughA) will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, keep the current value.
6. Repeat from step 2. 

What data structures do we need to implement Dijkstra's algorithm?

- unvisited nodes: min-heap. 

```
pseudo code of Dijkstra(graph, source, destination):
    create vertex set Q
    
    for each vertex v in Graph:
        v->dist = INFINITY
        v->prev = UNDEFINED
        v->visited = FALSE
        add v to Q
        
    source->dist = 0
    
    while Q is not empty:
        if all vertex in Q have dist INFINITY end the algorithm
        u = min dist vertex in Q
        remove u from Q
        u->visited = 1
        if u == destination:
            return u->dist
        
        for each neighbor v of u:
            if (!v->visited)
                alt = u->dist + dist(u, v)
                if alt < v->dist:
                    v->dist = alt
                    v->prev = u
    return INFINITY
        
```

Question: What is the time complexity of Dijkstra? 

Depends on your implementation. 
With priority queue and adjacency list, update the distance values costs O(V), we go through every edge, and we do find_min at most V times. It will be O((E + V)logV)

## Union find

check whether two nodes are connected 

```
function MakeSet(x)
   if x is not already present:
     add x to the disjoint-set tree
     x.parent := x
     x.rank   := 0
```

```
function Find(x)
   if x.parent != x
     x.parent := Find(x.parent)
   return x.parent
```

```
function Union(x, y)
   xRoot := Find(x)
   yRoot := Find(y)
 
   if xRoot == yRoot            
       return
 
   if xRoot.rank < yRoot.rank
     xRoot, yRoot := yRoot, xRoot // swap xRoot and yRoot
 
   yRoot.parent := xRoot
   if xRoot.rank == yRoot.rank:
     xRoot.rank := xRoot.rank + 1
```


```c
#include<stdio.h>
const int n = 10;
int parent[n];
int rank[n];
    
int find(int x){
    if (parent[x] != x){
        parent[x] = find(parent[x]);
    }
    return parent[x];
}

void swap(int* x, int* y){
    int tmp = *x;
    *x = *y;
    *y = tmp;
}

void make_union(int x, int y){
    int x_root = find(x);
    int y_root = find(y);
    
    if (x_root == y_root)
        return;
    
    if (rank[x_root] < rank[y_root]){
        swap(&x_root, &y_root);
    }
    
    parent[y_root] = x_root;
    if (rank[x_root] == rank[y_root]){
        rank[x_root]++;
    }
}

void check_connected(int x, int y){
    if (find(x) == find(y))
        printf("%d and %d are connected\n", x, y);
    else
        printf("%d and %d are not connected\n", x, y);
}

int main(){
   
    for (int i = 0; i < n; ++i){
        parent[i] = i;
        rank[i] = 0;
    }
    
    make_union(0,1);
    make_union(1,2);
    make_union(2,3);
    make_union(3,0);
    make_union(0,7);
    make_union(5,6);
    make_union(9,6);
    make_union(8,4);
    make_union(8,2);
    
    
    check_connected(4,0);
    check_connected(4,1);
    check_connected(8,2);
    check_connected(1,6);
    
}
```

    4 and 0 are connected
    4 and 1 are connected
    8 and 2 are connected
    1 and 6 are not connected


## Minimum Spanning Trees

In graph theory, a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any acyclic connected graph is a tree. 

- A spanning tree of a graph is a subgraph that contains all the vertices and is a tree. 
- For a weighted graph, the minimum spanning tree is the spanning tree with minimum total weights. 

Lemma: Let X be any subset of the vertices of G, and let edge e be the smallest edge connecting X to G-X. Then e is part of the minimum spanning tree.

Proof: Suppose you have a tree T not containing e; then I want to show that T is not the MST. Let e=(u,v), with u in X and v not in X. Then because T is a spanning tree it contains a unique path from u to v, which together with e forms a cycle in G. This path has to include another edge f connecting X to G-X. T+e-f is another spanning tree (it has the same number of edges, and remains connected since you can replace any path containing f by one going the other way around the cycle). It has smaller weight than t since e has smaller weight than f. So T was not minimum, which is what we wanted to prove.

### Kruskal's algorithm

```
inputs: G = (V, E)

Kruskal's algorithm:
    sort the edges of G in increasing order by length
    keep a subgraph S of G, initially empty
    for each edge e in E in sorted order:
        if the endpoints of e are disconnected in S:
            add e to S
    return S
```

To test whether the endpoints of e are disconnected, use Union Find. 

## Max Flow



```
Augment(f, c, p):
    b = bottleneck capacity of path P
    for each edge e in P:
        if e is an edge, f(e) = f(e) + b
        else f(e^R) = f(e^R) - b
    return f
```


```
Algorithm Ford-Fulkerson
Inputs: G = (V, E) with flow capacity c, a source node s, and a sink node t. 
Output: Compute a flow f from s to t of maximum value. 
    f(u, v) = 0 for all edges e = (u, v)
    G_f = residual graph
    while (there exists and augmenting path P in G_f):
        f = augment(f, c, P)
        update G_f
    return f
```
