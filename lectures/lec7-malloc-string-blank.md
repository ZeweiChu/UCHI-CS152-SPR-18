
# Lecture 7: Malloc, String
Zewei Chu 4/9/2018

### [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html)
(dynamically) allocate some new memories in the heap

- [Stack and Heap](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)
- [void*](https://en.wikipedia.org/wiki/Void_type) type
- array type not assignable

The following program won't work, because memory allocated on stack will be deallocated after the function returns. 


```c
#include<stdio.h>
#include<stdlib.h>
int main(){
    float arr[20];
    float* p = (float*)malloc(20*sizeof(float));
    p++;
    //arr++;
    return 0;
}
```


```c
#include<stdio.h>
double* alloc_init_final_grades(int num_students){
    double scores[num_students];
    for (int i = 0; i < num_students; i++)
        scores[i] = (double)i;
    return scores;
}
int main(){
    double* scores = alloc_init_final_grades(20);
    for (int i = 0; i < 20; i++)
        printf("%lf ", scores[i]);
    return 0;
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmp2kjfzypx.c:6:12: warning: address of stack memory associated with local variable 'scores' returned [-Wreturn-stack-address]
        return scores;
               ^~~~~~
    1 warning generated.


    0.000000 0.000000 0.000000 0.000000 -321.468709 0.000000 0.500000 0.000000 10.000000 0.000000 0.000000 0.000000 10.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 

- Do not return a pointer to a local variable - the memory will have been deallocated by then. 
- Same reason, do not return an array. 
- Use [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) instead to allocate memory on the heap. [malloc](http://pubs.opengroup.org/onlinepubs/009695399/functions/malloc.html) allocates memory in bytes and returns a pointer pointing to the beginning of the space. If memory allocation failed, a NULL pointer is returned.  


```c
#include<stdio.h>
#include<stdlib.h>

double* alloc_init_final_grades(int num_students){
    double* scores = (double*)malloc(num_students*sizeof(double));
    for (int i = 0; i < num_students; i++)
        scores[i] = (double)i;
    return scores;
}
int main(){
    double* scores = alloc_init_final_grades(20);
    for (int i = 0; i < 20; i++)
        printf("%lf ", scores[i]);
    free(scores);
    return 0;
}
```

    0.000000 1.000000 2.000000 3.000000 4.000000 5.000000 6.000000 7.000000 8.000000 9.000000 10.000000 11.000000 12.000000 13.000000 14.000000 15.000000 16.000000 17.000000 18.000000 19.000000 

### Dynamically allocated 2-D array


```c
#include<stdio.h>
double init_2d_values(int height, int width)[3][5]{
    double arr[3][5];
    for (int i = 0; i < 3; i++){
        for (int j = 0; j<5; ++j)
            arr[i][j] = i*j;
    }
    return arr;
}

int main(){
    double arr[3][5]; // = {{0.1, 0.2, 0.3, 1.2, 1.5}, {-1, -2, -3, -4.5, -9}, {1,2,3,4,5}};
    arr = init_2d_values(3,5);
    for (int i = 0; i < 3; i++){
        for (int j = 0; j<5; ++j)
            printf("%lf ", arr[i][j]);
        printf("\n");
    }
    return 0;
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpj_d3zvpx.c:2:22: error: function cannot return array type 'double [3][5]'
    double init_2d_values(int height, int width)[3][5]{
                         ^
    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpj_d3zvpx.c:8:12: warning: incompatible pointer to integer conversion returning 'double [3][5]' from a function with result type 'int' [-Wint-conversion]
        return arr;
               ^~~
    1 warning and 1 error generated.
    [C kernel] GCC exited with code 1, the executable will not be executed


```c
#include<stdio.h>
#include<stdlib.h>
double** create_and_init_2d(int height, int width){
    double** array2d = (double**)malloc(height*sizeof(double*));
    for (int i = 0; i < height; i++){
        array2d[i] = malloc(width*sizeof(double));
        for (int j = 0; j < width; j++)
            array2d[i][j] = j*i;
    }
    return array2d;
}

void free_2d(double** array, int height){
    
    
    for (int i = 0; i < height; i++)
        free(array[i]);
    
    free(array);
    
    
}

void print_2d_array(double** array, int height, int width){
}

int main(){
    //double array2d[3][5];
    double** array2d = create_and_init_2d(3, 5);
    printf("%lf \n", array2d[2][4]);
    free_2d(array2d, 3);
    return 0;
}
```

    8.000000 


### String
- Array of characters ending in '\\0' (a null character). 


```c
#include<stdio.h>
int main(){
    char a[20] = "abcde"; // abcde \0
    printf("%s\n", a);
    printf("%lu\n", sizeof(a));
    printf("%lu\n", sizeof("abcde"));
    
}
```

    abcde
    20
    6


- Need to #include<string.h> for string library functions
- strcpy function to copy a string


```c
#include<stdio.h>

int main(){

    return 0;
}
```

    hello
    5
    hello


```c
#include<stdio.h>
#include<string.h>

void strcpy2(char dest_str[], char source_str[]){
}

int main(){
    return 0;
}
```

    hello
    5
    hello

- strlen to count string length


```c
#include<stdio.h>
#include<string.h>

int strlen2(char str[]){
}

int main(){
    return 0;
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpdold2q1q.c:5:1: warning: control reaches end of non-void function [-Wreturn-type]
    }
    ^
    1 warning generated.


### command line arguments
- argc: number of arguments
- argv: arguments - an array of strings 


```c
#include<stdio.h>

int main(int argc, char* argv[]){
    return 0;
}
```

    1
    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpw64_cfqf.out 

