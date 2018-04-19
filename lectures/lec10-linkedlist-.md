
# Lecture 10: Linked list
Zewei Chu 4/16/2018

### Announcement
Exam 1 is this Friday, 4/20, 2:30 in this classroom

## Linked List

A linked list is a linear data structure where each element is a separate object. Each element (we will call it a node) of a list is comprising of two items - the data and a reference to the next node. The last node has a reference to null. The entry point into a linked list is called the head of the list.


```c
#include<stdio.h>
typedef struct _node node;
struct _node{
    int value;
    node *next;
};

void print_intlist(node* p){
    while (p != NULL){
        printf("%d ", p->value);
        p = p->next;
    }
}
int main(){
    node *p1, *p2, *p3, *p4;
    node l1, l2, l3, l4;
    p1 = &l1;
    p2 = &l2;
    p3 = &l3;
    p4 = &l4;
    
    p1->value = 1;
    p2->value = 2;
    p3->value = 3;
    p4->value = 4;
    
    //l1.next = p2;
    //(*p1).next = p2;
        
    p1->next = p2;
    p2->next = p3;
    p3->next = p4;
    p4->next = NULL;
    
    print_intlist(p1);
    return 0;
}

```

    1 2 3 4 

- Usually we use malloc to dynamically create linked list. 
- free the linked list at the end to prevent memory leak.

### Linked List Inspection


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
    printf("node: %d\n", p->value);
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
    while (head != NULL){
        tmp = head;
        head = head->next;
        printf("freeing node %d\n", tmp->value);
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

// return the first element of the linked list
node* first(node* head){
    if (is_empty(head)){
        raise_error("empty linked list", -1);
    }
    return head;
}

node* last(node* head){
    if (is_empty(head)){
        raise_error("empty linked list", -1);
    }
    while (head->next != NULL){
        head = head->next;
    }
    return head;
}

// return all but the first element in the linked list
node* rest(node* head){
    if (is_empty(head)){
        raise_error("empty linked list", -1);
    }
    return head->next;
}

// return sum of the int linked list
int sum(node* head){
    int res = 0;
    for (; head!= NULL; head = head->next)
        res += head->value;
    return res;
}

// return sum of the int linked list with recursion
int sum_rec(node* head){
    if (head == NULL)
        return 0;
    return head->value + sum_rec(head->next);
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

// create a linked list containing length nodes
// without using push_front


int main(){
    node* head = (node*)malloc(sizeof(node));
    head->value = 0;
    node* curr = head;
    curr->next = (node*)malloc(sizeof(node));
    curr = curr->next;
    curr->value = 1;
    
    curr->next = (node*)malloc(sizeof(node));
    curr = curr->next;
    curr->value = 2;
    curr->next = NULL;
    
    print_intlist(head);
    printf("is the linked list empty? %d\n", is_empty(head));
    printf("is the linked list empty? %d\n", is_empty(curr->next));
    
    print_node(first(head));
    printf("last node: ");
    print_node(last(head));
    
    print_intlist(rest(head));
    
    printf("sum of the linked list: %d\n", sum(head));
    printf("sum recursion of the linked list: %d\n", sum_rec(head));
    
    head = push_front(4, head);
    print_intlist(head);
    
    head = push_back(5, head);
    print_intlist(head);
    
    node* head2 = node_create(100);
    
    printf("free linked list.....\n");
    free_intlist(head);
    
    
    
    return 0;
}

```

    node: 0
    node: 1
    node: 2
    is the linked list empty? 0
    is the linked list empty? 1
    node: 0
    last node: node: 2
    node: 1
    node: 2
    sum of the linked list: 3
    sum recursion of the linked list: 3
    node: 4
    node: 0
    node: 1
    node: 2
    node: 4
    node: 0
    node: 1
    node: 2
    node: 5
    free linked list.....
    freeing node 4
    freeing node 0
    freeing node 1
    freeing node 2
    freeing node 5


Advantages of linked list

- Items can be added or removed from the middle of the list
- There is no need to define an initial size

Disadvantages 

- There is no "random" access - it is impossible to reach the nth item in the array without first iterating over all items up until that item. This means we have to start from the beginning of the list and count how many times we advance in the list until we get to the desired item.
- Dynamic memory allocation and pointers are required, which complicates the code and increases the risk of memory leaks and segment faults.
- Linked lists have a much larger overhead over arrays, since linked list items are dynamically allocated (which is less efficient in memory usage) and each item in the list also must store an additional pointer.
