
# Binary/Hex, Bitwise Operations

Zewei Chu

## Binary

- binary: base 2
- octal: base 8
- hex: base 16

These number systems translate easily because their bases are powers of 2

exercise 1

- 0b01001101001010111010000100101101
- 0o11512720455
- 0x4d2ba12d 

exercise 2

- 0b10010110
- 0o226
- 0xe6

exercise 3

- 0o346
- 0b011100110
- 0xe6

exercise 4

- 0x154
- 0b000101010100
- 0o0524

## Signed 2's complement notation

- Addition
- Subtraction
- Overflow

[Reference](https://www.cs.cornell.edu/~tomf/notes/cps104/twoscomp.html)


```c
#include<stdio.h>
int main(){
    int a = -1;
    long int b = -1;
    printf("%x\n", a);
    printf("%lx\n", b);
    
    printf("%x\n", 1^2);
    
    int x = 0xaa;
    int y = 0xdb;
    printf("%d %d\n", x, y);
    printf("%x\n", x&y);
    printf("%x\n", x|y);
    printf("%x\n", x^y);
    printf("%x\n", ~x);
    
    printf("%d\n", 10>>2);
    printf("%d\n", -10>>2);
    printf("%d\n", 3<<1);
    printf("%d\n", (-3) << 1);
    
    
}
```

    /var/folders/87/k3tmbndn0b77_wxs0bbd_n4h0000gq/T/tmpea8yjqwf.c:21:25: warning: shifting a negative signed value is undefined [-Wshift-negative-value]
        printf("%d\n", (-3) << 1);
                       ~~~~ ^
    1 warning generated.


    ffffffff
    ffffffffffffffff
    3
    170 219
    8a
    fb
    71
    ffffff55
    2
    -3
    6
    -6


&&

## Bitwise Operators

- op1 & op2 -- the bitwise AND operator 
- op1 | op2 -- the bitwise OR operator
- op1 ^ op2 -- the bitwise EXCLUSIVE-OR operator
- ~op1 -- the bitwise COMPLEMENT operator
- op1 >> op2 -- the SHIFT RIGHT operator moves the bits to the right, discards the far right bit, and pad leftmost with the most significant bit. 
- op1 << op2 -- the SHIFT LEFT operator moves the bits to the left, descards the far left bit, and pad rightmost with 0s. 

## Multiplication, Division

- 7 << 1 == 14
- -5 << 2 == -20
- 14 >> 1 == 7
- -20 >> 2 == -5

## Masking single bits


```c
#include<stdio.h>
#define MAST_1 0x1
#define MAST_11 0x3
#define MAST_10 0x2
#define MASK_100 0x4

int main(){
    int x;
    x = 7 & MAST_11;
    printf("%d\n", x);
    x = 11 & MAST_11;
    printf("%d\n", x);
    x = 15 & MAST_11;
    printf("%d\n", x);
    x = 19 & MAST_11;
    printf("%d\n", x);
    x = 17 & MAST_11;
    printf("%d\n", x);

}
```

    3
    3
    3
    3
    1


## Exercise: packing data into a single integer

Problem: We have a lot of variables, and we want to communicate them to a machine  in the shortest message possible. We would like to use the same integer for multiple, small variables. 

x: 5 bits
y: 7 bits
z: 8 bits

We will lay it out in this way: z y x

There are three types of operations, extraction, compaction and modification. 


```c
#include<stdio.h>
#define MAST_1 0x1
#define MAST_11 0x3
#define MAST_10 0x2
#define MASK_100 0x4
#define MASK_11111 0x1f
#define MASK_1111111 0x7f
#define MASK_11111111 0xff

int main(){
    int x = -5;
    int y = -9;
    int z = -22;
    int var = ((x & MASK_11111) | ((y & MASK_1111111) << 5) | ((z & MASK_11111111)) << 12);
    printf("var: %d\n", var);
    printf("var: %x\n", var);
    printf("var: %o\n", var);
    
    x = (var & 0x1f);
    if (x & 0x10){
        x |= ~(0x1f);
    }
    
    printf("x: %d\n", x);
    y = (var & 0xfff) >> 5;
    if (y & 0x7f){
        y |= ~(0x7f);
    }
    
    // y = y | (- ((x & 0x7f) >> 6)) << 6;
    printf("y: %d\n", y);
    z = (var & 0xfffff) >> 12;
    if (z & 0xff){
        z |= ~(0xff);
    }
    
    //z = z | (- ((x & 0xff) >> 7)) << 7;
    printf("z: %d\n", z);
}
```

    var: 962299
    var: eaefb
    var: 3527373
    x: -5
    y: -9
    z: -22

