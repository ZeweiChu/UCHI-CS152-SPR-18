
# Lecture 11: Linked List, Stack, Queue
Zewei Chu 4/18/2018

### Announcement
Exam 1 is this Friday, 4/20, 2:30 in this classroom

## Linked List

A linked list is a linear data structure where each element is a separate object. Each element (we will call it a node) of a list is comprising of two items - the data and a reference to the next node. The last node has a reference to null. The entry point into a linked list is called the head of the list.

- Usually we use malloc to dynamically create linked list. 
- Free the linked list at the end of the program to prevent memory leak.
- Always free the node when you remove a node from the linked list

### Linked List Operations


```c
#include<stdio.h>
#include<stdlib.h>
typedef struct _node node;
struct _node{
    int value;
    node* next;
};

// print information of a node
void print_node(node* p){
    printf("node: %d \n", p->value);
}

// print the whole int list
void print_intlist(node* head){
    if (head == NULL){
        return;
    } 
    print_node(head);
    print_intlist(head->next);
}

// free the whole int list
void free_intlist(node* head){
    node* tmp = head;
    printf("freeing linked list\n");
    while (head != NULL){
        tmp = head;
        head = head->next;
        free(tmp);
    }
}

// raise an error
void raise_error(char* msg, int code){
    printf("%s\n", msg);
    exit(code);
}

// check if the linked list is empty
int is_empty(node* head){
    return head == NULL;
}

// add v to the beginning of the linked list
// return the head node pointer
node* push_front(int v, node* head){
    node* newnode = (node*)malloc(sizeof(node));
    newnode->value = v;
    newnode->next = head;
    return newnode;
}

node* push_back(int v, node* head){
    node* tmp = head;
    node* newnode = (node*)malloc(sizeof(node));
    newnode->value = v;
    newnode->next = NULL;
    if (is_empty(head)){
        return newnode;
    }
    for (;tmp->next != NULL; tmp = tmp->next)
        ;
    tmp->next = newnode;
    return head;
}


// node_create – create a list containing length nodes. 
// Return pointer to first node. 
// Write iteratively using push_front
node* node_create(int length){
    if (length == 0){
        return NULL;
    }
    node* head = (node*)malloc(sizeof(node));
    head->value = 0;
    head->next = NULL;
    node* tmp = head;
    for (int i = 1; i < length; i++){
        tmp->next = (node*)malloc(sizeof(node));
        tmp->value = 0;
        tmp = tmp->next;
    }
    tmp->next = NULL;
    return head;
}


// create a new node
node* new_node(int v){
    node* p = (node*)malloc(sizeof(node)); //void*
    p->value = v;
    p->next = NULL;
    return p;
}

void insert_node_after(node* p, node* newnode){
    newnode->next = p->next;
    p->next = newnode;
}

// insert a node of value v at position pos
// returns the new head node
node* insert_node_at_pos(int v, node* head, int pos){
    node* newnode = new_node(v);
    if (is_empty(head)){
        return newnode;
    }
    if (pos == 0){
        newnode -> next = head;
        return newnode;
    }
    node* curr = head;
    int i = 1;
    for (; curr->next != NULL && i < pos; i++, curr = curr->next)
        ;
    insert_node_after(curr, newnode);
    return head;
}

// remove a node at position pos, 
// return the new head node pointer
node* remove_node_at_pos(node* head, int pos){
    if (is_empty(head)){
        return NULL;
    }
    node* curr = head;
    if (pos == 0){
        head = head->next;
        free(curr);
        return head;
    }
    
    //loop here
    int i = 1;
    for (; curr != NULL && i < pos; i++)
        curr = curr->next;
    
    if (curr == NULL || curr->next == NULL)
        return head;
    
    node* tmp = curr->next;
    curr->next = curr->next->next;
    free(tmp);
    return head;
}


int main(){
    int n = 10;
    node* head = node_create(n);
    int i = 0;
    node* curr = head;
    for (i = 0; i < n; ++i){
        curr->value = i;
        curr = curr->next;
    }
    // insert node
//     head = insert_node_at_pos(5, head, 0);
//     head = insert_node_at_pos(6, head, 1);
//     head = insert_node_at_pos(7, head, 6);
//     head = insert_node_at_pos(8, head, 8);
//     head = insert_node_at_pos(9, head, 100);
    
    head = remove_node_at_pos(head, 8);
    head = remove_node_at_pos(head, 8);
    head = remove_node_at_pos(head, 8);
    head = remove_node_at_pos(head, 100);
    head = remove_node_at_pos(head, 0);
//     head = remove_node_at_pos(head, -1);
    
    print_intlist(head);
    
    printf("free linked list.....\n");
    free_intlist(head);
    return 0;
}

```

    node: 1 
    node: 3 
    node: 4 
    node: 5 
    node: 6 
    node: 7 
    free linked list.....
    freeing linked list


Advantages of linked list

- Items can be added or removed from the middle of the list
- There is no need to define an initial size

Disadvantages 

- There is no "random" access - it is impossible to reach the nth item in the array without first iterating over all items up until that item. This means we have to start from the beginning of the list and count how many times we advance in the list until we get to the desired item.
- Dynamic memory allocation and pointers are required, which complicates the code and increases the risk of memory leaks and segment faults.
- Linked lists have a much larger overhead over arrays, since linked list items are dynamically allocated (which is less efficient in memory usage) and each item in the list also must store an additional pointer.

## Queue

Queue is a linear data structure which follows First In First Out (FIFO) operations. A queue has the same idea of consumers for products where the consumer that came first gets served first. 

A queue supports four basic operations: 

- Equeue: add an item to the queue
- Dequeue: remove and item from the queue
- Front: get the front item from the queue
- Rear: get the last item from the queue

reference:

- https://www.geeksforgeeks.org/queue-set-1introduction-and-array-implementation/
- https://www.geeksforgeeks.org/queue-set-2-linked-list-implementation/



```c
#include<stdio.h>
#include<stdlib.h>

// structures

//create a node

// create a queue

// print QNode

// print the queue

//enQueue

//deQueue

//fre


int main(){
    
    return 0;
}
```

## Stack

A stack is a First In Last Out (FILO) data structure. 

It should have three basic operations

- push: add an item in the stack
- pop: remove an element from the stack
- isEmpty: returns 1 if stack is empty, else 0


```c
#include <stdio.h>
#include <stdlib.h>
 
//structures
 
 
// raise an error
void raise_error(char* msg, int code){
    printf("%s\n", msg);
    exit(code);
}

// create a new node with value v


// create a new empty stack 

//isEmpty
 

// push a value into stack

// pop value out of stack

//free memory


int main(){
    
    return 0;
}

```

    poped value: 3
    poped value: 2
    poped value: 1
    Freeing stack...
