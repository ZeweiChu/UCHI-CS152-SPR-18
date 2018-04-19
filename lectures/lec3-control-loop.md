# Lecture 3
Zewei Chu, 3/30/2018

## Something new
- char type
```C
char b = 'a';
printf("%c", b);
```
- long, unsigned long
```C
unsigned long n = 1111111;
long m = -111111;
printf("%lu %ld", n, m);
```
- ++, --, +=, -=, \*=, \=
```
int a = 10;
a += 2;
a++; 
a--;
a\=2;
```



## Control flow


- if statement and logical operators
```C
void printScore(double score){
  if (score >= 90) {
    printf("A\n");
  } 
  if (score >= 80 && score < 90) {
    printf("B\n");
  }
  if (score >= 70 && score < 80) {
    printf("C\n");
  }
  if (score >= 60 && score < 70) {
    printf("D\n");
  }
  if (score < 60) {
    printf("F\n");
  }
}
```

- if else statement
```C
void printScore_ifelse(int score){
  if (score >= 90) {
    printf("A\n");
  } else if (score >= 80) {
    printf("B\n");
  } else if (score >= 70) {
    printf("C\n");
  } else if (score >= 60) {
    printf("D\n");
  } else {
    printf("F\n");
  }
}
```

- switch statement
```C
void printScore(double score){
  int tens = (int)(score/10);
  switch (tens){
    case (10): 
    case (9): printf("A\n"); break;
    case (8): printf("B\n"); break;
    case (7): printf("C\n"); break;
    case (6): printf("D\n"); break;
    deafult: printf("F\n");
  }
}
```



## Resources
- [C cheatsheet](https://courses.cs.washington.edu/courses/cse351/14sp/sections/1/Cheatsheet-c.pdf)
- [Precedence table](http://web.cse.ohio-state.edu/~babic.1/COperatorPrecedenceTable.pdf)
- [ASCII](https://www-s.acm.illinois.edu/webmonkeys/book/c_guide/a.html)





