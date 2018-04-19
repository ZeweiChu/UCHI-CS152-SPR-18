
# Lecture 6: Pointers, Character Array, String, Malloc
Zewei Chu, 4/6/2018

### Swap two variables


```c
#include<stdio.h>
void swap(int* x, int* y){
    int tmp = *x;
    *x = *y;
    *y = tmp;
}

int main(){
    int x = 2, y = 3;
    printf("x: %d, y: %d\n", x, y);
    swap(&x, &y);
    printf("x: %d, y: %d\n", x, y);
}
```

    x: 2, y: 3
    x: 3, y: 2


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
    int scores[20];
    for (int i = 0; i < 20; ++i) 
        scores[i] = (i*20 + 100) / 20 * 90 - 100;
    printf("%d\n", scores[5]);
    printf("%d\n", *(scores+5));
    printf("%d\n", scores[6]);
    printf("%d\n", *(scores+6));
}
```

    800
    800
    890
    890


### String
- Array of characters ending in '\0' (a null character). 


```c
#include<stdio.h>
int main(){
    printf("%s\n", "abcde");
}
```

    abcde


- Need to #include<string.h> for string library functions


```c
#include<stdio.h>
#include<string.h>

int main(){
    char myString[20];
    strcpy(myString, "hello");
    printf("%s\n", myString);
    printf("%lu\n", strlen(myString));
    for (int i = 0; i < 5; ++i)
        putchar(myString[i]);
    return 0;
}
```

    hello
    5
    hello

#### implement our own strcpy function


```c
#include<stdio.h>
#include<string.h>

void strcpy2(char dest_str[], char source_str[]){
    int i = 0;
    while (source_str[i] != '\0'){
        dest_str[i] = source_str[i];
        i ++;
    }
    dest_str[i] = '\0';
}

int main(){
    char myString[20];
    strcpy2(myString, "hello");
    printf("%s\n", myString);
    printf("%lu\n", strlen(myString));
    for (int i = 0; i < 5; ++i)
        putchar(myString[i]);
    return 0;
}
```

    hello
    5
    hello


```c
#include<stdio.h>

int strlen2(char str[]){
    int length = 0;
    while (str[length] != '\0')
        length ++;
    return length;
}

int main(){
    printf("%d\n", strlen2("hello"));
    return 0;
}
```

    5


### [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html)
(dynamically) allocate some new memories in the heap

- [Stack and Heap](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)

The following program won't work, because memory allocated on stack will be deallocated after the function returns. 


```c
#include<stdio.h>
double* alloc_init_final_grades(int num_students){
    double scores[num_students];
    for (int i = 0; i < num_students; i++)
        scores[i] = 2.0;
    return scores;
}
```

- Do not return a pointer to a local variable - the memory will have been deallocated by then. 
- Same reason, do not return an array. 
- Use [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) instead to allocate memory on the heap. [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) allocates memory in bytes and returns a pointer pointing to the beginning of the space. If memory allocation failed, a NULL pointer is returned.  


```c
#include<stdio.h>
#include<stdlib.h>

double* alloc_init_final_grades(int num_students){
    double* scores = (double*) malloc(sizeof(double) * num_students); 
    for (int i = 0; i < num_students; i++)
        scores[i] = 2.0;
    return scores;
}

int main(){
    double * scores = alloc_init_final_grades(10);
    scores[2] = 95;
    scores[5] = 75;
    *(scores+1) = 88.5;
    for (int i = 0; i < 10; i++)
        printf("%.2lf ", *(scores+i));
    printf("\n");
    free(scores);
    return 0;
}


```

    2.00 88.50 95.00 2.00 2.00 75.00 2.00 2.00 2.00 2.00 

