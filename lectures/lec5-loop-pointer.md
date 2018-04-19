
# Nested Loops
Zewei Chu, 4/4/2018
- Loops can be nested (for, while)


```c
#include<stdio.h>
int main(){
    int height=3, width=5;
    for (int i = 0; i < height; ++i){
        for (int j = 0; j < width; ++j){
            printf("*");
        }
        printf("\n");
    }
    return 0;
}
```

    *****
    *****
    *****


Question: How can we draw a triangle? 

2-D arrays
- create a 2-D array for students' scores of assignments
- compute average score for each assignment


```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

#define NUM_STUDENTS 30
#define NUM_ASSIGNS 13 //10 weeks of assignments, 3 exams

int main(){
    double scores[NUM_STUDENTS][NUM_ASSIGNS];
    double avg_scores[NUM_ASSIGNS];
    memset(avg_scores, 0, sizeof(double)*NUM_ASSIGNS);
    
    // except the index to iterate, all other variables 
    // need to have a meaningful name, 
    // use either camal case or underscore
    for (int i = 0; i < NUM_STUDENTS; i++){
        for (int j = 0; j < NUM_ASSIGNS; j++){
            scores[i][j] = (i * j + 250) % 100;
        }
    }
    
    for (int j = 0; j < NUM_ASSIGNS; j++){
        for (int i = 0; i < NUM_STUDENTS; i++){
        //    printf("%lf ", scores[i][j]);
            avg_scores[j] += scores[i][j]; 
        }
        avg_scores[j] /= NUM_STUDENTS;
        printf("average score of assignment %d is: %lf", j, avg_scores[j]);
        printf("\n");
    }
}
```

    average score of assignment 0 is: 50.000000
    average score of assignment 1 is: 64.500000
    average score of assignment 2 is: 62.333333
    average score of assignment 3 is: 50.166667
    average score of assignment 4 is: 51.333333
    average score of assignment 5 is: 55.833333
    average score of assignment 6 is: 50.333333
    average score of assignment 7 is: 51.500000
    average score of assignment 8 is: 52.666667
    average score of assignment 9 is: 50.500000
    average score of assignment 10 is: 45.000000
    average score of assignment 11 is: 49.500000
    average score of assignment 12 is: 54.000000


# Pointers
- a variable pointing to another variable (memory address)
- \* and & operators


```c
#include<stdio.h>
void print(int value){
    printf("%d\n", value);
}
int main(){
    int a = 1, b=2, c=3;
    print(a);
    print(b);
    print(c);
    printf("%p", &a);
}
```

    1
    2
    3
    0x7ffeeaab59bc

## swap two variable


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

