
# Week 5 Lec 1

Zewei Chu

## Enumeration Type



```c
int main(){
    int day;
    day = 0;
    
}
```


```c
#include<stdio.h>
enum _weekday {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
typedef enum _weekday weekday;
enum boolean {FALSE, TRUE};
int main(){
    weekday day;
    day = Mon;
    printf("%d\n", day);
    
    day = Wed;
    printf("%d\n", day);
    
    day = Fri;
    printf("%d\n", day);
    
    if (day < Sat){
        printf("weekday\n");
    }
    
    day = Sun;
    printf("%d\n", day);
}
```

    1
    3
    5
    weekday
    0


## Union Type

- Store different data types in the same memory location. 
- You can define a union with many members, but only one member can contain a value at any given time.


```c
#include <stdio.h>
#include <string.h>
 
union Data {
    int i;
    float f;
    char str[20];
    double l[1000];
};
 
int main( ) {

    union Data data;        

    printf("size of data: %lu\n", sizeof(data));
    printf("size of int: %lu\n", sizeof(int));
    printf("size of float: %lu\n", sizeof(float));
    printf("size of char: %lu\n", sizeof(char));
    data.i = 10;
    data.f = 220.5;
    strcpy( data.str, "C Programming");
    data.i = 10;

    printf( "data.i : %d\n", data.i);
    printf( "data.f : %f\n", data.f);
    printf( "data.str : %s\n", data.str);

    return 0;
}
```

    size of data: 8000
    size of int: 4
    size of float: 4
    size of char: 1
    data.i : 10
    data.f : 0.000000
    data.str : 
    


### An exercise using union and enum types


```c
#include<stdio.h>
#include<math.h>

typedef struct point {
    float x, y;
} point_t;

typedef struct circle {
    point_t center;
    float radius;
} circle_t;

typedef struct triangle {
    point_t a, b, c;
} triangle_t;

enum shape_tag {
    CIRCLE, TRIANGLE
};

union _shape {
    circle_t c;
    triangle_t t;    
}; 

typedef struct shape{
    union _shape sh;
    enum shape_tag tag;
} shape_t;

float dist(point_t a, point_t b){
    return sqrt((a.x-b.x)*(a.x-b.x) + (a.y-b.y)*(a.y-b.y));
}

float perimeter(shape_t* s){
    if (s->tag == CIRCLE){
        return 2*M_PI*s->sh.c.radius;
    } else {
        return dist(s->sh.t.a, s->sh.t.b) + dist(s->sh.t.a, s->sh.t.c) 
            + dist(s->sh.t.b, s->sh.t.c);
    }
}

int main(){
    circle_t c = {{0, 1.2}, 2.5};
    
    shape_t s1 = {{c}, CIRCLE};
    printf("perimeter of s1: %f\n", perimeter(&s1));
    
    triangle_t t1 = {{0.0, 0.0}, {0.0, 3.0}, {4.0, 3.0}};
    shape_t s2;
    s2.tag = TRIANGLE;
    s2.sh.t = t1;
    printf("perimeter of s1: %f\n", perimeter(&s2));
    
    triangle_t t2 = {{0.0, 1}, {2, 3.0}, {4.0, 3.0}};
    s2.sh.t = t2;
    printf("perimeter of s2: %f\n", perimeter(&s2));
    
    return 0;
}
```

    perimeter of s1: 15.707963
    perimeter of s1: 12.000000
    perimeter of s2: 9.300563



```c
#include<stdio.h>
union types {
    double a;
    int b;
    float c;
    char d;
    char arr[20];
};
int main(){
    union types u;
    u.a = 2.0;
    printf("%lf\n", u.a);
    printf("%f\n", u.c);
    printf("%d\n", u.b);
    printf("%s\n", u.arr);
}
```

    2.000000
    0.000000
    0
    



```c
#include<stdio.h>
int first(char* str, char c){
    if (*str == '\0')
        return -1;
    if (*str == c)
        return 0;
    str++;
    int pos = first(str, c);
    if (pos == -1)
        return -1;
    else
        return 1+pos;
}
int main(){
    printf("%d\n", "abcdef", 'e');
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmp4aglt8qo.c:15:20: warning: format specifies type 'int' but the argument has type 'char *' [-Wformat]
        printf("%d\n", "abcdef", 'e');
                ~~     ^~~~~~~~
                %s
    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmp4aglt8qo.c:15:30: warning: data argument not used by format string [-Wformat-extra-args]
        printf("%d\n", "abcdef", 'e');
               ~~~~~~            ^
    2 warnings generated.


    29101990

