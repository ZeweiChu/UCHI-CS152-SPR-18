
## Priority Queue

- A priority queue is an abstract data type, where each element has a "priority" associated with it.
- In a priority queue, an element with high priority is served before an element with low priority.

A priority queue must at least support the following operations: 

- is_empty
- enqueue - add an element
- dequeue - find the largest element 

To decide the "priority" of an element, we need a comparison function $int (*compare)(void*, void*)$

We could implement this as a linked list

- enqueue - add element to the head O(1)
- dequeue - use the compare method on the elements in the queue to find the "largest" one O(n)

Another implementation with linked list

- enqueue - place the item in order into the linked list based on compare O(n)
- dequeue - return the first item in the list O(1)

We want a faster implementation - one that gets O(logn) instead of log(n)

- Can we use BST?

## Heap

A heap is a tree based data structure that satisfies the heap property:

- if P is a parent node of C, then the value of P is either greater than or equal to (in a max heap) or less than or equal to (in a min heap) the value of C. 

We focus on max heap in this class. 

### Max Heap

- satisfies the properties of a priority queue - insert & remove, but the removal always returns the highest priority element. 
- order property - left tree < node && right tree < node
- shape property - tree is a complete binary tree (A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible. )

Insert an item

- maintains the shape property - insert into the next leaf available, i.e., adding from left to right in the lowest level. 
- fix the order property - bubbleUp - compare the new node to its parent. If new node > parent, swap. Repeat until new node < parent OR new node == root. 

remove an item

- maintains the shape property - place the rightmost leaf of the lowest level into the root
- fix the order property - bubbleDown - compare moved node with its children. If the larger of the two children is larger than the moved node, swap. Repeat until moved node is larger than its children OR it is a leaf. 

### Array Implementation of Max Heap

- The root element is at arr[0]
- For arr[i], the parent node is arr[(i-1)/2], the left child is arr[2*i+1], the right child is arr[2*i+2]. 


```c
#include<stdio.h>
#include<stdlib.h>
#include<limits.h>

typedef unsigned int uint;

typedef struct{
    uint size;
    uint capacity;
    int arr[];
}maxheap;

maxheap* maxheap_new(uint capacity){
    maxheap* h = (maxheap*)malloc(sizeof(maxheap) + capacity * sizeof(int));
    h->size = 0;
    h->capacity = capacity;
    //h->arr = (int*)malloc(sizeof(int)*capacity);
    return h;
}


void bubbleUp(maxheap* h){
    uint index = h->size-1;
    while (index != 0 && h->arr[(index-1)/2] < h->arr[index]){
        int tmp = h->arr[(index-1)/2];
        h->arr[(index-1)/2] = h->arr[index];
        h->arr[index] = tmp;
        index = (index-1) / 2;
    }
}

void enqueue(maxheap* h, int value){
    if (h->size >= h->capacity) {
        h->capacity *= 2;
        h = (maxheap*)realloc(h, sizeof(maxheap) + h->capacity * sizeof(int));
    }
    h->arr[h->size++] = value;
    printf("heap size: %u\n", h->size);
    bubbleUp(h);
}

void bubbleDown(maxheap* h, uint start){
    uint index = start;
    while (index*2+1 < h->size){ // has left child
   
        int max_child = index*2 + 1;
        if (index*2+2 < h->size && h->arr[index*2+1] 
            < h->arr[index*2+2]){ // has right child
            max_child = index*2+2;
        } 
        if (h->arr[index] < h->arr[max_child]){
            int tmp = h->arr[index];
            h->arr[index] = h->arr[max_child];
            h->arr[max_child] = tmp;  
            index = max_child;
        } else {
            break;
        }
    }
}


int dequeue(maxheap* h){
    if (h->size == 0){
        fprintf(stderr, "the heap is empty!");
        return INT_MIN;
    }
    int ret = h->arr[0];
    h->arr[0] = h->arr[h->size-- - 1];
    bubbleDown(h, 0);
    return ret;
}

int main(){
    maxheap* h = maxheap_new(4);
    enqueue(h, 10);
    enqueue(h, 5);
    enqueue(h, 20);
    enqueue(h, 45);
    enqueue(h, 15);
    enqueue(h, 50);
    enqueue(h, 60);
    
    printf("dequeued value %d\n", dequeue(h));
    printf("dequeued value %d\n", dequeue(h));
    printf("dequeued value %d\n", dequeue(h));
    enqueue(h, 22);
    enqueue(h, 30);
    enqueue(h, 25);
    printf("dequeued value %d\n", dequeue(h));
    printf("dequeued value %d\n", dequeue(h));
    printf("dequeued value %d\n", dequeue(h));
}



```

    heap size: 1
    heap size: 2
    heap size: 3
    heap size: 4
    heap size: 5
    heap size: 5
    heap size: 6
    dequeued value 60
    dequeued value 50
    dequeued value 45
    heap size: 4
    heap size: 5
    heap size: 6
    dequeued value 30
    dequeued value 25
    dequeued value 22

