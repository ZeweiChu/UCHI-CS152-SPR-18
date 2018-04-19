
- hw1 due today
- office hours, Tuesday 10-12, Eckhart 131

# Nested Loops
Zewei Chu, 4/4/2018
- Loops can be nested (for, while). Draw a rectangle



```c
*****
*****
*****
```


```c
#include<stdio.h>
int main(){
    int x = 4; 
    int length;
    int i;
    
    for (i = 0; i < x; i++){
        length = 4-i;
        while (length > 0){
            printf("*"); 
            length--;
        }
        printf("\n");
    }
}
```

    ****
    ***
    **
    *


Question: How can we draw a triangle? 


```c
****
***
**
*
```

2-D arrays
- create a 2-D array for students' scores of assignments
- compute average score for each assignment


```c
#include<stdio.h>
#define NUM_STUDENTS 30
#define NUM_ASSIGNS 13

int main(){
    double scores[NUM_STUDENTS][NUM_ASSIGNS];
    //initialize the 2-D array
    for (int i = 0; i < NUM_STUDENTS; i++){
        for (int j = 0; j < NUM_ASSIGNS; j++){
            scores[i][j] = (i + 20*j + 200 + 10*i)%100;
        }
        //printf("\n");
    }
    
    scores[0][0] = 90;
    scores[1][1] = 85;
    
    double avg_scores[NUM_ASSIGNS];
    for (int j = 0; j < NUM_ASSIGNS; j++){
        // sum the score of assignment j
        avg_scores[j] = 0;
        for (int i = 0; i < NUM_STUDENTS; i++){
            // add student i's score to avg_scores[j]
            avg_scores[j] += scores[i][j];
        }
        avg_scores[j] /= NUM_STUDENTS;
        printf("average score of assignment %d: %lf\n", j, avg_scores[j]);
    }
    return 0;
}
```

    average score of assignment 0: 52.500000
    average score of assignment 1: 51.300000
    average score of assignment 2: 49.500000
    average score of assignment 3: 49.500000
    average score of assignment 4: 49.500000
    average score of assignment 5: 49.500000
    average score of assignment 6: 49.500000
    average score of assignment 7: 49.500000
    average score of assignment 8: 49.500000
    average score of assignment 9: 49.500000
    average score of assignment 10: 49.500000
    average score of assignment 11: 49.500000
    average score of assignment 12: 49.500000


# Pointers
- a variable pointing to another variable (memory address)
- \* and & operators


```c
#include<stdio.h>

int main(){
    int a = 3;
    
    int *p;
    p = &a;
    printf("%p\n", p);
    printf("%p\n", &p);
    printf("%d\n", *p); // dereference
    a = 6;
    printf("%d\n", *p);
}
```

    0x7ffeebe979bc
    0x7ffeebe979b0
    3
    6


## swap two variable
