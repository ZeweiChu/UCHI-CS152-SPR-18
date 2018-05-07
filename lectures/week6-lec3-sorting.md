
# Sorting algorithms

Zewei Chu

- hw6 deadline this Friday
- Exam2 on Monday, 5/14, in class. 


```c
#include<stdio.h>
#include<stdlib.h>

void swap(int* a, int* b){
    int tmp;
    tmp = *a;
    *a = *b;
    *b = tmp;
}

void selectionSort(int arr[], int n){
    int min_index;
    for (int i = 0; i < n; i ++){
        min_index = i;
        for (int j = i; j < n; j++){
            if (arr[j] < arr[min_index])
                min_index = j;
        }
        // swap the min_index element with the first element
        swap(&arr[i], &arr[min_index]);
    }
}

void bubbleSort(int arr[], int n){
    for (int i = 0; i < n-1; i++){ // n-1 iteration
        for (int j = 0; j < n-i-1; j++){ // n-i-1 iterations
            if (arr[j] > arr[j+1]){
                swap(&arr[j], &arr[j+1]);
            }
        }
    }
}

void merge(int arr[], int n, int* tmpArr){
    int half = n / 2;
    // 0, 2, 8, 12, 15
    // -22, -1, 10, 11, 18
    int i1 = 0, i2 = half;
    int index = 0;
    while (i1<half && i2 <n){
        if (arr[i1] < arr[i2]){
            tmpArr[index++] = arr[i1++];
        } else {
            tmpArr[index++] = arr[i2++];
        }
    }
    while (i2 < n){
        tmpArr[index++] = arr[i2++];
    }
    
    while (i1 < half){
        tmpArr[index++] = arr[i1++];
    }
    
    for (int i = 0; i < n; i++)
        arr[i] = tmpArr[i];
    
}

void mergeSort(int arr[], int n, int* tmpArr){
    if (n <= 1)
        return;
    int half = n / 2;
    mergeSort(arr, half, tmpArr);
    mergeSort(arr+half, n-half, tmpArr);
    
    //two sorted subarrays
    merge(arr, n, tmpArr); // 
}


int pivot_and_divide(int arr[], int n){
    int pivot = arr[n-1]; // get the pivot number
    int divide = 0;
    for (int i = 0; i<n-1; i++){
        if (arr[i] < pivot){
            swap(arr+i, &arr[divide]);
            divide++;
        }
    }
    swap(arr+divide, arr+n-1);
    return divide;
}

void quickSort(int arr[], int n){
    if (n <= 1) return;
    int pivot_position = pivot_and_divide(arr, n);
    quickSort(arr, pivot_position);
    quickSort(arr+pivot_position+1, n-pivot_position-1);
}

void printArray(int arr[], int n){
    for (int i = 0; i < n; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(){
    int arr[] = {12, 15, 0, 2, 8, -1, -22, 18, 11, 10, 15, -20, -15};
    int n = sizeof(arr) / sizeof(arr[0]);
//     int* tmpArr = malloc(sizeof(int)*n);
    // sort the array
    quickSort(arr, n);
//     free(tmpArr);
    
    printArray(arr, n);
}
```

    -22 -20 -15 -1 0 2 8 10 11 12 15 15 18 


## Heap Sort


```c
#include<stdio.h>

// recursive implementation
void bubbleDown(int arr[], int n, int i){
    int max_child = 2*i+1;
    if (max_child >= n) return;
    if (2*i+2 < n && arr[2*i+2] > arr[max_child])
        max_child = 2*i+2;
    if (arr[i] < arr[max_child]){
        int tmp = arr[i];
        arr[i] = arr[max_child];
        arr[max_child] = tmp;
        bubbleDown(arr, n, max_child);
    }    
}

void heapSort(int arr[], int n){
    for (int i = (n-1)/2; i >= 0; i--)
        bubbleDown(arr, n, i);
    int tmp;
    for (int i = n-1; i >= 0; i--){
        tmp = arr[0];
        arr[0] = arr[i];
        arr[i] = tmp;
        bubbleDown(arr, i, 0);
    }
}


void printArray(int arr[], int n){
    for (int i = 0; i < n; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(){
    int arr[] = {12, 15, 0, 2, 8, -1, -22, 18, 11, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    heapSort(arr, n);
    printArray(arr, n);
}
```

    -22 -1 0 2 8 10 11 12 15 18 


## Bubble Sort and Selection Sort


```c
#include<stdio.h>

void bubbleSort(int arr[], int n){
    int i, j, tmp;
    for (i = 0; i < n-1; ++i){
        for (j = 0; j < n-i-1; j++){
            if (arr[j] > arr[j+1]){
                tmp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = tmp;
            }
        }
    }
}

void selectionSort(int arr[], int n){
    int i, j, min_index, tmp;
    for (i = 0; i < n-1; i++){
        min_index = i;
        for (j = i+1; j < n; ++j){
            if (arr[j] < arr[min_index])
                min_index = j;
        }
        tmp = arr[min_index];
        arr[min_index] = arr[i];
        arr[i] = tmp;
    }
}

void printArray(int arr[], int n){
    for (int i = 0; i < n; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(){
    int arr[] = {12, 15, 0, 2, 8, -1, -22, 18};
    int n = sizeof(arr) / sizeof(arr[0]);
    selectionSort(arr, n);
    printArray(arr, n);
}
```

    -22 -1 0 2 8 12 15 18 


## Merge Sort


```c
#include<stdio.h>
void merge(int arr[], int n, int tmpArr[]){
    int half = n/2;
    int i1 = 0;
    int i2 = half;
    int index = 0;
    while (i1 < half && i2 < n){
        if (arr[i1] < arr[i2])
            tmpArr[index++] = arr[i1++];
        else
            tmpArr[index++] = arr[i2++]; 
    }
    while (i1 < half)
        tmpArr[index++] = arr[i1++];
    while (i2 < n)
        tmpArr[index++] = arr[i2++];
        
    index = 0;
    while (index < n){
        arr[index] = tmpArr[index];
        index++;
    }
}

void mergeSort(int arr[], int n, int tmpArr[]){
    if (n <= 1)
        return;
    int half = n / 2;
    mergeSort(arr, half, tmpArr);
    mergeSort(arr+half, n-half, tmpArr);
    merge(arr, n, tmpArr);
}

void printArray(int arr[], int n){
    for (int i = 0; i < n; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(){
    int arr[] = {12, 15, 0, 2, 8, -1, -22, 18};
    int n = sizeof(arr) / sizeof(arr[0]);
    int tmpArr[n];
    mergeSort(arr, n, tmpArr);
    printArray(arr, n);
}
```

    -22 -1 0 2 8 12 15 18 


## Quick Sort


```c
#include<stdio.h>

int pivot_and_divide(int arr[], int n){
    int pivot = arr[n-1];
    int divide = 0;
    int tmp;
    for (int i = 0; i < n-1; ++i){
        if (arr[i] < pivot){
            tmp = arr[i];
            arr[i] = arr[divide];
            arr[divide++] = tmp;
        }
    }
    arr[n-1] = arr[divide];
    arr[divide] = pivot;
    return divide;
}

void quick_sort(int arr[], int n){
    if (n <= 1)
        return;
    int pivot = pivot_and_divide(arr, n);
    quick_sort(arr, pivot);
    quick_sort(arr+pivot+1, n-pivot-1);
}

void printArray(int arr[], int n){
    for (int i = 0; i < n; ++i)
        printf("%d ", arr[i]);
    printf("\n");
}

int main(){
    int arr[] = {12, 15, 0, 2, 8, -1, -22, 18};
    int n = sizeof(arr) / sizeof(arr[0]);
    quick_sort(arr, n);
    printArray(arr, n);
}
```

    -22 -1 0 2 8 12 15 18 

