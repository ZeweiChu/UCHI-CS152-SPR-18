
Review of Hash Table, hash map

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


```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

typedef struct {
    char* key; //key is the city name
    int value; //value is the index of the city
}HashEntry;

typedef struct {
    HashEntry* entries;
    int size;
    int (*hash)(char* key, int size);
}HashTable;

int char_hash(char* key, int size){
    int res = 17;
    for (int i = 0; i < strlen(key); ++i)
        res = (res*37+key[i]) % size;
    return res;
}

HashTable* create_table(int size, 
                        int (*hash)(char*, int)){
    HashTable* ht = (HashTable*)malloc(sizeof(HashTable));
    ht->entries = (HashEntry*)malloc(size*sizeof(HashEntry));
    for (int i = 0; i < size; ++i){
        ht->entries[i].key = NULL;
    }
    ht->size = size;
    ht->hash = hash;
    return ht;
}

void hash_insert(HashTable* ht, char* key, int value){
    int index = ht->hash(key, ht->size);
    int probe = 0;
    while (ht->entries[index+probe].key != NULL){
        probe++;
    }
    ht->entries[index+probe].key = key;
    ht->entries[index+probe].value = value;
}

int hash_search(HashTable* ht, char* key){
    int index = ht->hash(key, ht->size);
    int probe = 0;
    while (ht->entries[index+probe].key == NULL){
        probe++;
    }
    if (strcmp(ht->entries[index+probe].key, key)==0){
        return ht->entries[index+probe].value;
    }
    return -1;
}

typedef struct _neighbor{
    char* name;
    struct _neighbor* next;
}neighbor;

typedef struct {
    char* name;
    neighbor* neighbors;
    int visited;
}vertex;

typedef struct {
    vertex** cities;
}graph;

vertex* vertex_new(char* name){
    vertex* r = (vertex*)malloc(sizeof(vertex));
    r->name = strdup(name);
    r->neighbors = NULL;
    return r;
}

void vertex_insert(graph* g, char* city, int index, HashTable* ht){
    hash_insert(ht, city, index);
    vertex* newcity = vertex_new(city);
    g->cities[index] = newcity;
}

neighbor* neighbor_new(char* name){
    neighbor* n = (neighbor*)malloc(sizeof(neighbor));
    n->name = strdup(name);
    n->next = NULL;
    return n;
}

void neighbor_insert(vertex* v, char* name){
    neighbor* n = neighbor_new(name);
    n->next = v->neighbors;
    v->neighbors = n;
}

void vertex_insert_neighbor(graph* g, char* v, char* w, HashTable* ht){
    int iv = hash_search(ht, v);
    neighbor_insert(g->cities[iv], w);
}

graph* graph_new(){
    graph* g = (graph*)malloc(sizeof(graph));
    g->cities = (vertex**)malloc(sizeof(vertex*)*20);
    return g;
}

graph* init_graph(HashTable* ht){
    
    graph* g = graph_new();
    vertex_insert(g, "SF", 0, ht);
    vertex_insert(g, "LA", 1, ht);
    vertex_insert(g, "CHI", 2, ht);
    vertex_insert(g, "Houston", 3, ht);
    vertex_insert(g, "NYC", 4, ht);
    vertex_insert(g, "DC", 5, ht);
    
    vertex_insert_neighbor(g, "SF", "CHI", ht);
    vertex_insert_neighbor(g, "CHI", "Houston", ht);
    vertex_insert_neighbor(g, "Houston", "NYC", ht);
    vertex_insert_neighbor(g, "Houston", "DC", ht);
    vertex_insert_neighbor(g, "DC", "SF", ht);
    vertex_insert_neighbor(g, "LA", "NYC", ht);
    vertex_insert_neighbor(g, "NYC", "LA", ht);
    
    
    return g;
    
    
}

void init_dfs(graph* g){
    for (int i = 0; i < 20; i++){
        if (g->cities[i]){
            g->cities[i]->visited = 0;
        }
    }   
}

int dfs(graph* g, char* v, char* dest, HashTable* ht){
    int index = hash_search(ht, v);
    g->cities[index]->visited = 1;
    if (strcmp(v, dest) == 0)
        return 1;
    
    neighbor* n = g->cities[index]->neighbors;
    while (n){
        char* name = n->name;
        index = hash_search(ht, name);
        if (!g->cities[index]->visited){
            if (dfs(g, name, dest, ht)){
                return 1;
            }
        }
        n = n->next;
    }
    
    return 0;
}


int main(){
    HashTable* ht = create_table(20, char_hash);
    graph* g = init_graph(ht);
    
    init_dfs(g);
    int reachable = dfs(g, "SF", "NYC", ht);
    
    init_dfs(g);
    reachable = dfs(g, "SF", "LA", ht);
    
    init_dfs(g);
    reachable = dfs(g, "LA", "SF", ht);
    
    printf("reachable: %d\n", reachable);

    return 0;
}


```

    reachable: 0

