
## Computational complexity of data structures

| Data structure        | insert           | remove  | find |
| ------------- |:-------------:| -----:|-----:|
| stack      | O(1) | O(1) |  |
| queue      | O(1)      |   O(1) |  |
| BST | O(logn)      |    O(logn) | O(logn) |
|heap| O(logn) | O(logn) |  |

Question: can we do insert/find/remove in O(1)? 

Yes, we can do that with a hash table. Hash tables are built with arrays. What? An array? Then why did we spend the whole quarter learning linked structures if the most efficient structure is an array? 

- Part of the reason is that I want you ro learn program with pointers. 
- Also because there are trade-offs to implement all operations in O(1) time complexity. 

In our explorations of hash tables, here are the three questions we need to focus on:

- How is it that, with an array, we could get O(1) in inser/find/remove?
- How can we guarentee O(1) in the average case? 
- What are we giving up to obtain O(1)? (no free lunch)

## Example of student records

Let's take a concrete example. We want to store the records of all students at the university. Each student has one unique identifier that is 9 digits long. We only need to store 10^4 student records at any one time. The only thing that we care about are insert, delete and find. Is there a way that I could store them suh that I could access them in O(1) time? 

- Array with 10^9 slots, index is the ID number - constant lookup, but space is 10^9. 
- BST, but time complexity is O(log n)
- hash table - array with capacity related to 10^4, but lookup on average O(1)

Let's think about a more tractable example. We have 10 student records we want to store, and an array of size 20 to store them in. What are the design challenges? We don't know the index of the 10 student records in the array. 

- Given a student record, what is the array index to store that record? - Hash Function. 
- If two student records map to the same location, how do we resolve it? - Collision resolution techniques. 

## The hash function


#### Hash function maps from item to index in the array

The hash function decides which index to look for an item. Every piece of data needs to have some unique information that we use for the hash function. This is called the "key". The hash function is a function that receives as input that "key" and outputs an index in the hash table. Therefore, the signature of the hash function is: 

```C
int hash(int key, int size)
```

In our student record case, the simplest hash function is 
```C
int hash(int idnumber, int size){
    return idnumber % size;
}                         
```

How about this hash function? 
```C
int hash(int idnumber, int size){
    return 8;
}    
```

#### A good hash function spreads out items in the table

With the mod hash function, can you create a set of student IDs that all maps to the same index? 

Does this make mod a bad hash function? 

Not necessarily. Any function that operates well on random data is potentially a good function. At the same time, all hash function have a set of keys that will make them perform poorly. So, no matter how "good" it is, we can construct a scenario to hurt it. 

#### Any hash function can perform poorly depending on the inputs. Hash function performance is a relationship between hash function and the set of keys of items hashed in the table. 

## Collisions

What causes collisions? 

- size of hash table
- hash function itself
- distribution of keys
- numbr of keys

### How do we resolve collisions? 

- Linear probing. problems: clustering, removal. 
- Quadratic probing. 
- Double hashing. Use anohter hash function as probing step size. 
- Linked list collision resolution. 


### Let's put them together

Insert the following student records: 

- 12, A
- 23, B 
- 11, C
- 22, D
- 43, E 
- 44, F
- 25, G
- 65, H 
- 77, I
- 78, J

### More considerations

What do we do when we run out of array space? Double the size of the hash table.

What happens when we double the size of the hash table? All items must be reinserted into the table. 


```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

enum status_t {FILLED, UNFILLED, DELETED};

typedef struct {
    int key;
    void* data;
    enum status_t status;
}HashEntry;

typedef struct {
    HashEntry* table;
    int size;
    int (*hash)(int key, int size);
}HashTable;

int mod_hash(int key, int size){
    return key % size;
}

HashTable* create_table(int size, 
                        int (*hash)(int, int)){
    HashTable* ht = (HashTable*)malloc(sizeof(HashTable));
    ht->table = (HashEntry*)malloc(size*sizeof(HashEntry));
    for (int i = 0; i < size; ++i)
        ht->table[i].status = UNFILLED;
    ht->size = size;
    ht->hash = hash;
    return ht;
}

// HashEntry* hash_entry_new(int key, void* data){
//     HashEntry* e = (HashEntry*)malloc(sizeof(HashEntry));
//     e->key = key;
//     e->data = data;
//     e->status = FILLED;
//     return e;
// }

void insert(HashTable* ht, int key, void* data){
    int index = ht->hash(key, ht->size);
    int probe = 0;
    int i = 1;
    while (ht->table[index+probe].status == FILLED){
//         probe++;
        probe += i*i;
        i++;
    }
    ht->table[index+probe].key = key;
    ht->table[index+probe].data = data;
    ht->table[index+probe].status = FILLED;
}

void* find(HashTable* ht, int key){
    int index = ht->hash(key, ht->size);
    int probe = 0;
    while (ht->table[index+probe].status != UNFILLED){
        if (ht->table[index+probe].key == key){
            return ht->table[index+probe].data;
        }
        probe++;
    }
    return NULL;
}

void delete(HashTable* ht, int key){
    int index = ht->hash(key, ht->size);
    int probe = 0;
    while (ht->table[index+probe].status != UNFILLED){
        if (ht->table[index+probe].status == FILLED &&
            ht->table[index+probe].key == key){
            free(ht->table[index+probe].data);
            ht->table[index+probe].data = NULL;
            ht->table[index+probe].status = DELETED;
            return;
        }
        probe++;
    }
}

typedef struct{
    int id;
    char name[50];
}student;

void print_student(student* s){
    if (!s) {
        printf("record NULL\n");
        return;
    }
    printf("student id: %d, name: %s\n", s->id, s->name);
}

int main(){
    student* a = (student*)malloc(sizeof(student));
    a->id = 12345;
    strcpy(a->name, "Zewei");
    
    student* b = (student*)malloc(sizeof(student));
    b->id = 22546;
    strcpy(b->name, "Diana");
    
    student* c = (student*)malloc(sizeof(student));
    c->id = 55875;
    strcpy(c->name, "David");
    
    student* d = (student*)malloc(sizeof(student));
    d->id = 55985;
    strcpy(d->name, "Tri");
    
    HashTable* ht = create_table(50, mod_hash);
    insert(ht, a->id, a);
    insert(ht, b->id, b);
    insert(ht, c->id, c);
    insert(ht, d->id, d);
    
    delete(ht, 55985);
    student* p = (student*)find(ht, 55875);
    
    print_student(p);
    p = (student*)find(ht, 55985);
    print_student(p);
    delete(ht, 55875);
     p = (student*)find(ht, 55875);
    print_student(p);
}
```

    student id: 55875, name: David
    record NULL
    record NULL

