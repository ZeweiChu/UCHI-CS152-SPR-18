
# Week 6 Lecture 1

Zewei Chu


```c
count = 0;
for (i = 0; i < M; i++){
    count++;
}
```

## Algorithmic complexity

The complexity of the following code

```C
int count = 0;
for (int i = 0; i < N; i++)
    for (int j = 0; j < i; j++)
        count++;

for (int i = 0; i < N; i+=2)
    count++;
```

Another example

```C
int count = 0;
for (int i = N; i >= 0; i /= 2)
    for (int j = 0; j < i; j++)
        count++;
```

Think about the time complexity of the following algorithm
```C
long long rec(int n){
    if (n == 1){
        return 2;
    }
    return rec(n-1) + rec(n-1);
}
```


```c
#include<stdio.h>

long long rec(int n){
    if (n == 1){
        return 2;
    }
    return rec(n-1) + rec(n-1);
}

int main(){
    printf("%lld\n", rec(40));
    return 0;
}
```


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
