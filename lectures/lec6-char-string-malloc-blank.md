
# Lecture 6: Pointers, Character Array, String, Malloc
Zewei Chu, 4/6/2018

### Swap two variables


```c
#include<stdio.h>
void swap(int* a, int* b){
    printf("a = %p, b = %p\n", a, b);
    printf("*a = %d, *b = %d\n", *a, *b);
    int* tmp = a;
    a = b;
    b = tmp;
    printf("a = %p, b = %p\n", a, b);
    printf("*a = %d, *b = %d\n", *a, *b);
}

int main(){
    int x = 2, y = 3;
    printf("before function call: x = %d, y = %d\n", x, y);
    swap(&x, &y);
    printf("after function call: x = %d, y = %d\n", x, y);
    return 0;
}
```

    before function call: x = 2, y = 3
    a = 0x7ffeea8ee998, b = 0x7ffeea8ee994
    *a = 2, *b = 3
    a = 0x7ffeea8ee994, b = 0x7ffeea8ee998
    *a = 3, *b = 2
    after function call: x = 2, y = 3


### Types

Types specify two things: the size and the interpretation of the numbers. 

| Relevant Type           | size   |
| -------------:| -----:|
| char | 1 byte |
| int       |   2 or 4 bytes |
| long | 4 bytes | |
|  float       |    4 bytes |
| double | 8 bytes| 


### Array and pointer are the same thing
- [reference](https://www.le.ac.uk/users/rjm1/cotter/page_59.htm)


```c
#include<stdio.h>
int main(){
    float scores[10];
    for (int i = 0; i < 10; i++){
        scores[i] = i*10;
    }
    printf("%f\n", scores[0]);
    printf("%f\n", *scores);
    printf("%f\n", scores[1]);
    printf("%f\n", *(scores+100));
    printf("%f\n", *(1+scores));
    //printf("%f\n", 1[scores]);
    
}
```

    0.000000
    0.000000
    10.000000
    -3050037201474259188992442368.000000
    10.000000


### String
- Array of characters ending in '\0' (a null character). 

- Need to #include<string.h> for string library functions

#### implement our own strcpy function

### [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html)
(dynamically) allocate some new memories in the heap

- [Stack and Heap](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)


```c
#include<stdio.h>
#include<stdlib.h>

int main(){
    printf("%lu\n", sizeof(double));
    double* array = (double*)malloc(100 * sizeof(double));
    double array2 = array;
    array[2] = 12;
    *(array+5) =25;
    printf("%lf\n", *(array+2));
    printf("%lf\n", array[5]);
    //free(array);
    free(array2);
}
```

    8
    12.000000
    25.000000


The following program won't work, because memory allocated on stack will be deallocated after the function returns. 

- Do not return a pointer to a local variable - the memory will have been deallocated by then. 
- Same reason, do not return an array. 
- Use [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) instead to allocate memory on the heap. [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) allocates memory in bytes and returns a pointer pointing to the beginning of the space. If memory allocation failed, a NULL pointer is returned.  
