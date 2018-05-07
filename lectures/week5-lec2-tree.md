
# Generic Linked List, Binary Tree

Zewei Chu

## Generic Linked List


```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

// generic node
typedef struct _node{
    void* value;
    struct _node* next;
}node;

typedef struct {
    node* head;
}list;

void push(node** headref, void* newdata, size_t datasize){
    node* newnode = (node*)malloc(sizeof(node));
    newnode->value = malloc(datasize);
    newnode->next = *headref;
    
    newnode->value = newdata;
    
    memcpy(newnode->value, newdata, datasize);
    *headref = newnode;
}

void* iterate(list* lst){
    static node* tmp = NULL;
    if (lst != NULL){
        tmp = lst->head;
    }
    if (tmp == NULL){
        return NULL;
    }
    void* retval = tmp->value;
    tmp = tmp->next;
    return retval;
}


typedef struct{
    int value;
}IntValue;

int main(){
    
    list* lst = (list*)malloc(sizeof(list));
    lst->head = NULL;
    
    IntValue* v = (IntValue*)malloc(sizeof(IntValue));
    v->value = 1;
    push(&(lst->head), v, sizeof(IntValue));
    
    v = (IntValue*)malloc(sizeof(IntValue));
    v->value = 2;
    push(&(lst->head), v, sizeof(IntValue));
    
    v = (IntValue*)malloc(sizeof(IntValue));
    v->value = 3;
    push(&(lst->head), v, sizeof(IntValue));
    
    IntValue* item;
    for (item = iterate(lst); item != NULL; item = iterate(NULL))
        printf("%d ", item->value);
    printf("\n");
    
    
    list* lst2 = (list*)malloc(sizeof(list));
    lst->head = NULL;
    
    float* f = (float*)malloc(sizeof(float));
    *f = 0.1;
    push(&(lst2->head), f, sizeof(float));
    
    f = (float*)malloc(sizeof(float));
    *f = 0.2;
    push(&(lst2->head), f, sizeof(float));
    
    f = (float*)malloc(sizeof(float));
    *f = 0.3;
    push(&(lst2->head), f, sizeof(float));
    
    float* item2;
    for (item2 = iterate(lst2); item2 != NULL; item2 = iterate(NULL))
        printf("%f ", *item2);
    printf("\n");
    
    return 0;
}
```

    3 2 1 
    0.300000 0.200000 0.100000 



```c
#include<stdio.h>


void demo(){
    static int i = 0;
    printf("%d\n", i);
    i++;
}

int main(){
    for (int i = 0; i < 10; i++)
        demo();
}
```

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9


## Binary Tree

- A node is a structure that may contain a value, a condition, or represent a separate data structure (which could be a tree of its own). 
- Each node in a tree has zero or more child nodes, which are below it in the tree (by convention, trees are drawn growing downwards). 
- A node that has a child is called the child's parent node. A node has at most one parent.
- An internal node or inner node is any node of a tree that has child nodes. Similarly, an external node (also known as an outer node, leaf node, or terminal node), is any node that does not have child nodes.
- The topmost node in a tree is called the root node. Being the topmost node, the root node will not have a parent. It is the node at which operations on the tree commonly begin (although some algorithms begin with the leaf nodes and work up ending at the root). All other nodes can be reached from it by following edges or links. (In the formal definition, each such path is also unique). In diagrams, it is typically drawn at the top
- The height of a node is the length of the longest downward path to a leaf from that node. The height of the root is the height of the tree. The depth of a node is the length of the path to its root (i.e., its root path).
- A subtree of a tree T is a tree consisting of a node in T and all of its descendants in T. The subtree corresponding to the root node is the entire tree;
- A binary tree is a tree data structure in which each node has at most two child nodes, usually distinguished as "left" and "right"


```c
#include<stdio.h>
#include<stdlib.h>
#include<limits.h>

struct _node{
    int value;
//     void* value;
    struct _node* left;
    struct _node* right;
};
typedef struct _node node;

// dynamically allocate memory to a new node
node* new_node(int v){
    node* p = (node*)malloc(sizeof(node));
    p->left = NULL;
    p->right = NULL;
    p->value = v;
    return p;
}

// compute the sum of all node values
int tree_sum(node* root){
    if (root == NULL){
        return 0;
    }
    return root->value + 
        tree_sum(root->left) + tree_sum(root->right);
}

// get the height of the tree
int tree_height(node* root){
    if (root == NULL){
        return 0;
    } 
    int lheight = tree_height(root->left);
    int rheight = tree_height(root->right);
    if (lheight > rheight){
        return lheight + 1;
    } else {
        return rheight + 1;
    }
}

// get the maximum value of the tree
int tree_max(node* root){
    if (root == NULL)
        return INT_MIN;
    int lmax = tree_max(root->left);
    int rmax = tree_max(root->right);
    int max = lmax>rmax?lmax:rmax;
    max = root->value>max?root->value:max;
    return max;
}

void print_node(node* n){
    printf("%d\t", n->value);
}

void inorder_traverse(node* t, void (*print)(void*)){
    if (t == NULL){
        return;
    }
    inorder_traverse(t->left, print);
    print(t);
    inorder_traverse(t->right, print);
}

int main(){
    node* root = new_node(5);
    root->left = new_node(4);
    root->left->left = new_node(3);
    root->right = new_node(8);
    root->right->left = new_node(6);
    root->right->right = new_node(9);
    
    printf("sum of all tree nodes: %d\n", tree_sum(root));
    printf("height of the tree: %d\n", tree_height(root));
    printf("max value of the tree: %d\n", tree_max(root));
    
    
    inorder_traverse(root, print_node);
    return 0;
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpmbv_20qx.c:81:28: warning: incompatible pointer types passing 'void (node *)' (aka 'void (struct _node *)') to parameter of type 'void (*)(void *)' [-Wincompatible-pointer-types]
        inorder_traverse(root, print_node);
                               ^~~~~~~~~~
    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpmbv_20qx.c:59:39: note: passing argument to parameter 'print' here
    void inorder_traverse(node* t, void (*print)(void*)){
                                          ^
    1 warning generated.


    sum of all tree nodes: 35
    height of the tree: 3
    max value of the tree: 9
    3	4	5	6	8	9	

###  Tree Traversals

A tree traversal is the order in which you visit the nodes. Depending on what data is in the tree and how it is organized, different orders make sense.


## Binary Search Tree

Binary search tree is an abstract data type. It has a defined interface with specific time  guarantees, though it can be implemented as either an array or a linked list. 

A binary search tree is a node based binary tree data structure. Each node contains two pointers pointing to other nodes, one to the left child node, the other to the right child node. 

- The left subtree of a node only contains nodes with keys less than (or equal to) the current node. 
- The right subtree of a node only contains nodes with keys greater than (or equal to) the current node. 
- Both the best and right subtrees must also be binary search trees. 

Based on the above definition of binary search tree, you need a definition of node and a comparison function of nodes to create a binary search tree. 


```c
void insert(bst*, void*);
int remove(bst*, void*);
void* find(bst*, void*);
void* interator(bst*);
int bst_is_empty(bst*);
```

### Algorithmic complexity

We need a formal way of calculating algorithmic complexity because intuition only goes so far.
Algorithmic complexity is an expression of how quickly the computation needs grow relative to the problem size.

How do we solve it?

- Determine what variable(s) indicate the problem size.
- Find the instruction that, at large problem sizes, will execute the most times.
- Write an equation for the number of times that instruction will be executed relative to the variables in (1).
- Drop all but the fastest-growing term.
- Drop any constant multiplicative factor

We have already done simple ones with linked lists – if you have to traverse the length of the linked list, then you’re doing n or n-1 operations. This reduces down to O(n).

How about recursive functions? You count number of function calls * cost of each call.

One last one: Fibonacci – fib(n) = fib(n-1) + fib(n-2)

Cost of each call: Constant time (no loops)

Number of function calls? Draw it out

But we can’t solve that – it’s equal to calculating the Fibonacci sequence.

So then it’s between fib(n) = fib(n-1) + fib(n-1) and fib(n) = fib(n-2) + fib(n-2)
Let’s do both of those.

- fib(n) = fib(n-1) + fib(n-1) + C = 2 * fib(n-1) + C = 2 * (2 * fib(n-2)+C)+C = 4*(fib(n-2)+C) +2C+C= 8(fib(n-3)+C)+4C+3C = 8* fib(n-3)+8C+7C = 2^n * fib(0) + (2^n-1)C
- fib(n) = fib(n-2) + fib(n-2) +C = 2 * fib(n-2)+C = 2*(2fib(n-4)+C)+ C = 4f(n-4)+3C = 4 * ( 2fib(n-6)+C)+3C= 8fib(n-6)+7C = 8*(2fib(n-8)+C)+7C = 16fib(n-8)+15C = 2^k*fib(n-2K)+(2^k- 1)C. 
K = n/2 when n-2K = 0.
= 2^(n/2) T(0)+(2^(n/2)-1)C 
(2^(n/2)-1) = 2^sqrt(n) – 1. Remove 1. 2^sqrt(n).

So we are bounded by 2^sqrt(n) and 2^n. When we do algorithmic complexity, we overestimate
rather than underestimate, so we determine it to be 2^sqrt(n).

Let’s try with an easier one – finding an item in a sorted array.

Starting from the beginning and checking each one until you find it.

- Best-case – 1
- Worst-case – n
- Average-case – n/2

How do we get average case?

probability found in a particular location, time to find it there. Sum those.
1/n * 1 + 1/n * 2 + 1/n * 3 + 1/n * 4, …, 1/n * n
= sum (1+2+…+n) / n = (((n+1)*n)/2)/n = (n^2/2 + n/2)/n = n/2 + 1/2

O(n)

