# Lecture 4
Zewei Chu, 4/2/2018, Monday

## Something new
- putchar
- puts
- const variable
- float



## Arrays
- define and initialize a float array
```C
float arr[] = {1.5, 2.5, 3.2, -0.8, 5.5};
```

## Loops

### for loop
- compute the sum of an array of float values
```C
float sum(float arr[], unsigned int size){
  float res = 0;
  for (unsigned int i = 0; i < size; ++i){
    res += arr[i];
  }
  return res;
}
```

- find the max value of an array of float
```C
float find_max(float arr[], unsigned int size){
  float max_val = arr[0];
  for (unsigned int i = 1; i < size; i++){
    if (arr[i] > max_val) 
      max_val = arr[i];
  }
  return max_val;
}
```

### while loops

- find the index of the first occurrence of a value in an array
```C
int find_index(float arr[], 
    unsigned int length, float value){
  int i = 0;
  while (i < length && fabs(arr[i] - value) > 1e-9) i++;
  if (i >= length) i = -1;
  return i;
}
```

- revisit the factorial computation problem
```C
unsigned long factorial(int n){
  int i = 1;
  unsigned res = 1;
  while (i <= n){
    res = res * i;
    i = i + 1;
  }
  return res;
}
```

- break
- continue

## Nested loops
- draw a rectangle of asterisks
```C
void drawRectangle(int height, int width){
  for (int i = 0; i < height; i++){
    for (int j = 0; j < width; j++){
      putchar('*');
    }
    putchar('\n');
  }
}
```
