
# Lecture 8, String, Structure
Zewei Chu 4/11/2018

## String
- Need to #include<string.h> for string library functions
- strcpy function to copy a string


```c
#include<stdio.h>
#include<string.h>

int main(){
    char s[20] = "hello";
    char s2[20] = {'h', 'e', 'l', 'l', 'o', '\0'};
    printf("%s\n", s);
    printf("%s\n", s2);
    printf("%lu\n", sizeof(s));
    printf("%lu\n", sizeof(s2));
    printf("%lu\n", strlen(s));
    printf("%lu\n", strlen(s2));
    
    
    return 0;
}
```

    hello
    hello
    20
    20
    5
    5


implement our own strlen function strlen2


```c
#include<stdio.h>
int strlen2(char* s){
    int i;
    for (i = 0; i < sizeof(s); i++){
        if (s[i] == '\0') break; 
    }
    return i;
}

int strlen3(char* s){
    int i = 0;
    while (s[i] != '\0'){
        i++;
    }
    return i;
}
int main(){
    char s[] = "abcdefg";
    printf("%d\n", strlen2(s));
    printf("%d\n", strlen3(s));
}
```

    7
    7


write our own strcpy function: strcpy2


```c
#include<stdio.h>
#include<string.h>

void strcpy2(char dest_str[], char source_str[]){
    int i = 0;
    while (source_str[i] != '\0'){
        dest_str[i] = source_str[i];
        i++;
    }
    dest_str[i] = '\0';
}

int main(){
    char a[] = "something";
    char b[20];
    printf("%s\n", a);
    printf("%s\n", b);
    strcpy2(b, a);
    printf("%s\n", b);
    return 0;
}
```

    something
    
    something



```c
#include<stdio.h>
int main(){
    char* s = "abcdefg";
    printf("%c\n", *(s+1));
    printf("%c\n", s[1]);
    s = s+2;
    printf("%c\n", s[1]);
    printf("%c\n", s[0]);
    printf("%c\n", s[-1]);
    
}
```

    b
    b
    d
    c
    b


- [strlen](http://pubs.opengroup.org/onlinepubs/009695399/functions/strlen.html) to count string length
- exercise: write our own strlen function - strlen2


```c
#include<stdio.h>
#include<string.h>

int strlen2(char str[]){
    
    return 0;
}

int main(){
    
    return 0;
}
```

More string [functions](https://www.tutorialspoint.com/c_standard_library/string_h.htm)

### command line string arguments
- argc: number of arguments
- argv: arguments - an array of strings 


```c
#include<stdio.h>

int main(int argc, char* argv[]){

    return 0;
}
```

### An exercise of pointers


```c
#include<stdio.h>

int main(){
    char* c[] = {"ENTER", "NEW", "POINT", "FIRST"};
    char** cp[] = {c+3, c+2, c+1, c};
    char*** cpp = cp;
    printf("%s\n", c[1]);
    printf("%s\n", *(c+1));
    printf("%s\n", c[0]);
    printf("%p\n", *(c));
    //char** cp[]
    printf("%s\n", *(*(cp+1)));
    printf("%s\n", *(*(cp+1) + 1));
    printf("%s\n", **(++cp));
    //printf("%s\n", *(-- *(++cp)) +3 );
    //printf("%s\n", *(-- *(++cpp)) +3 );
    //printf("%s\n", *(-- *(++cpp)) +3 );
    
    
    return 0;
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpgri8g3zi.c:14:23: error: cannot increment value of type 'char **[4]'
        printf("%s\n", **(++cp));
                          ^ ~~
    1 error generated.
    [C kernel] GCC exited with code 1, the executable will not be executed

## Struct
A structure is a collecton of one or more variables, possibly of different types, grouped together under a single name for convenient handling. 

Now lets define a structure for a student. We want the following information for a student:
- char name\[\]
- unsigned int studentID
- double scores\[\]


```c
#include<stdio.h>
#include<string.h>

int main(){
    
    return 0;
}
```


```c
#include<stdio.h>


int main(){
    return 0;
}
```
