
# Lecture 9: Structure, Linked list
Zewei Chu 4/13/2018

Some logical operators

- &&
- || 
- !

What is a NULL pointer?

A NULL pointer has a value reserved for indicating that the pointer does not refer to a valid object.


```c
#include<stdio.h>
int main(){
    int a = 3;
    int* p = NULL; //NULL;
    int* q = NULL;
    if (p!=q){
        printf("Not NULL");
    } else {
        printf("NULL");
    }
}
```

    NULL


```c
#include<stdio.h>
int main(){

    if (!(2 > 1) || 2 > 3){
        printf("True\n");
    } else {
        printf("False\n");
    }
    printf("%d\n", !0);
    return 0;
}
```

    False
    1


## Struct
A structure is a collecton of one or more variables, possibly of different types, grouped together under a single name for convenient handling. 

Now lets define a structure for a student. We want the following information for a student:
- char name\[\]
- unsigned int studentID
- double scores\[\]


```c
#include<stdio.h>
#include<string.h>

struct student_info {
    char name[50]; 
    unsigned int studentID; 
    double scores[13]; 
};

int main(){
    struct student_info s;
    printf("%lu\n", sizeof(s)); 
    strcpy(s.name, "Adam");
    s.studentID = 1;
    s.scores[0] = 90.5;
    s.scores[1] = 80;
    printf("%s, %u, %lf\n", s.name, s.studentID, s.scores[1]);
    
    struct student_info s2 = s;
    printf("%s, %u, %lf\n", s2.name, s2.studentID, s2.scores[1]);
    
    struct student_info arr[10];
    arr[0] = s;
    arr[1] = s;
    printf("%p %p\n", &(s.name), &(s2.name));
    
    printf("%s, %u, %lf\n", arr[0].name, arr[0].studentID, arr[0].scores[1]);
    return 0;
}
```

    160
    Adam, 1, 80.000000
    Adam, 1, 80.000000
    0x7ffeed323908 0x7ffeed323868
    Adam, 1, 80.000000



```c
#include<stdio.h>

struct point{
    int x;
    int y;
};

struct point makepoint(int x, int y){
    struct point p;
    p.x = x;
    p.y = y;
    return p;
}

int main(){
    struct point p; // = makepoint(2,3);
    p.x = 2;
    p.y = 3;
    struct point a = {2,3};
    printf("%d %d\n", a.x, a.y);
    return 0;
}
```

### typedef
typedef is a way to "rename" a type


```c
#include<stdio.h>
#include<string.h>
typedef unsigned int uint;
typedef struct{
    char name[50]; 
    uint studentID; 
    double scores[13]; 
}s_info;

int main(){
    s_info s;
    strcpy(s.name, "Adam");
    s.studentID = 1;
    s.scores[0] = 90.5;
    s.scores[1] = 80;
    printf("%s, %u, %lf\n", s.name, s.studentID, s.scores[1]);
    return 0;
}
```

    Adam, 1, 80.000000


### Where do I put my struct type declaration?
If multiple files use the struct type, declare the type in a .h file. 

### Functions with structs
- You can define functions to access (read/write) variables of structs


```c
#include<stdio.h>
#include<string.h>
typedef unsigned int uint;
typedef struct{
    char name[50]; 
    uint studentID; 
    double scores[13]; 
}s_info;

s_info make_s_info(char name[], uint sID){
    s_info s;
    strcpy(s.name, name);
    s.studentID = sID;
    return s;
}

void print_s_info(s_info s){
    printf("%s %u\n", s.name, s.studentID);
}


int main(){
    s_info s = make_s_info("Adam", 1);
    print_s_info(s);
    return 0;
}
```

    Adam 1


### Nested structures


```c
#include<stdio.h>
int main(){
    return 0;
}
```

## Linked List

A linked list is a linear data structure where each element is a separate object. Each element (we will call it a node) of a list is comprising of two items - the data and a reference to the next node. The last node has a reference to null. The entry point into a linked list is called the head of the list.


```c
#include<stdio.h>
typedef struct _intlist intlist;
struct _intlist{
    int value;
    intlist* next;
};
int main(){
    intlist l1, l2, l3, l4;
    
    l1.value = 1;
    l2.value = 2;
    l3.value = 3;
    l4.value = 4;
    
    l1.next = &l2;
    l2.next = &l3;
    l3.next = &l4;
    l4.next = NULL;
    
    printf("%d %d %d\n", l1.value, (*(l1.next)).value, (*((*(l1.next)).next)).value);
    
    
    return 0;
}

```

    1 2 3

