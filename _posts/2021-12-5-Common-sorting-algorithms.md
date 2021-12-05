---
title: "Common Sorting Algorithms"
date: 2021-12-5
categories:
  - blog
tags:
  - CS note 
excerpt: "seven sorting algorithms"
---

I am going to introduce and compare seven sorting algorithms that I learned from school. I will describe them with working logic, sudo codes, time complexity and extra memory need. The list of sorting algorithms is:

1. 1) straight selection sort
1. 2) bubble sort
2. 3) insertion sort
3. 4) merge sort
4. 5) quick sort
5. 6) heap sort
6. 7) radix sort

All of my code is available on [Github](https://github.com/hesy41/school-work/tree/main/data-structure-code/sorting_exercise).  

## 1. Straight Selection Sort

We first tag the first index in the array with “current position”. Then find the smallest element from “current position” to the last element in the array. Swap the “current position” with the smallest element. Move the “current position” tag to the next element. When the “current position” tag is passed to the last element in the array, the array is sorted.  

**Checker:** no use  
**Time complexity:** O(n^2)  
**In-place:** Yes  
**Sample process:**  
![Straight Selection Sort process](/assets/images/Straight-Selection-Sort-process.jpg)
**My code:**  

```cpp
{/*Straight Selection Sort*/
void sss(int a[], int size, int position);

void StraightSelectionSort(int arr[], int size)
{
    int current_position=0;
    sss(arr,size,current_position);
}

void sss(int a[], int size, int position)
{
    int smallest_position = position;

    if (position < size)
    {
        for (int i=position; i<size; i++)
        {
            if (a[i] < a[smallest_position])
                smallest_position = i;
        }

        swap(a[smallest_position], a[position]);
        position++;
        sss(a, size, position);
    }
}
/*---------------------------------------------------*/ 
```

## 2. Bubble Sort  

We first set the “current position” to the first index of the array. Then, set the last index to be “bubble center” and the one before it together to be a bubble. If the oder of the two element in bubble is right, move the “bubble center” forward. If not, swap the two elements in bubble then move the “bubble center” forward. Stop this bubble swapping when “bubble center” is the next index of the “current position”. Bubble swapping will bring the smallest value to the front. Then, move “current position” to the next index and then repeat the bubble swapping.  

**Checker:** during a bubble swapping, if no swapping is actually happening, then the sort is sorted. Function can terminate.  
**Time complexity:** O(n^2) best case O(n)  
**In-place:** yes  
**Sample process:**
![Bubble Sort process](/assets/images/bubble-sort-process.jpg)
**My code:**  

```cpp
/*bubble sort*/
void BubbleSort(int arr[], int size)
{
    bool swapping = false;
    for (int i=0; i<size; i++)
    {
        for (int j= size-1; j>i; j--)
        {
            if (arr[j]<arr[j-1])
            {
                swap(arr[j], arr[j-1]);
                swapping = true;
            }
        }
        if (swapping == false)
            return;
    }
}
```  

## 3. Insertion Sort

 insertion sort is actually to insert items in the right position. We have a “key” tag that is initially posed on the first item of the unsorted array. Compare the key value to its predecessor. If the key value is greater than its predecessor, insert the predecessor before the key; If not, the array stays the same. The part of the array before the key is sorted (including the key). And then we move “key” tag down to the next item in array. Compare predecessor of the key to the sorted part of the array and insert it in the right position. Move the “key” tag again until it reach the end.  
**Checker:** no use  
**Time complexity:** O(n^2) [best case: O(n) if the array is already sorted and no move is made]  
**In-place**: yes  
**Sample process:**  
![insertion sort process](/assets/images/Insertion-sort-process.jpg)
**My code:**

```cpp
/*insertion sort*/
void insert(int[], int size, int key);

void InsertionSort (int arr[], int size)
{
    for (int key=0; key<size; key++)
    {
        if (arr[key+1]<arr[key])
            insert(arr, size, key);
    }
}

void insert(int a[], int size, int key)
{
    int predecessor = a[key+1];

    while (predecessor < a[key] && key>=0)
    {
        swap (a[key+1], a[key]);
        key--;
    }
}
```

## 4. Merge Sort

We first spilt (divide) the array into half and recursively splitting it until the all the subarrays contain no more than two elements. And then merge (conquer) all the subarrays levels by levels from the bottom.  

**Checker:** no use  
**Time complexity:** O(N*log(N))  
**In-place:** no  
**Sample process:**
![merge sort process](/assets/images/merge-sort-process.jpg)  
**My code:**

```cpp
/*merge sort*/
void Spilt(int arr[], int head, int tail);
void Merge(int a[], int head, int mid, int tail);

void MergeSort(int arr[], int size)
{
    Spilt(arr, 0, size-1);
}

void Spilt(int arr[], int head, int tail)
{   
    //base case: stop when the subarray have one element
    if (head >= tail) 
        return;
    else 
    {
        int mid = (head+tail)/2;
        Spilt(arr, head, mid);
        Spilt(arr, mid+1,tail);
        Merge (arr, head, mid, tail);
    }
}

void Merge(int a[], int head, int mid, int tail)
{
    int subOneSize= mid - head +1;
    int subTwoSize=tail - mid;

    int* subOne= new int[subOneSize];
    int* subTwo = new int[subTwoSize];

    for (int i=0; i<subOneSize; i++) //copy data to the sub array
        subOne[i] = a[head + i];
    for (int j=0; j<subTwoSize; j++)
        subTwo[j] = a[mid+j+1];

    int indexOne = 0;
    int indextwo = 0;
    int indexMerged = head;

    while (indexOne<subOneSize && indextwo<subTwoSize)
    {
        if (subOne[indexOne]<subTwo[indextwo])
        {
            a[indexMerged] = subOne[indexOne];
            indexOne++;
            indexMerged++;
        }
        else
        {
            a[indexMerged] = subTwo[indextwo];
            indextwo++;
            indexMerged++;
        }
    }
    while (indexOne<subOneSize) 
    {
        a[indexMerged] = subOne[indexOne];
        indexOne++;
        indexMerged++; 
    }

    while (indextwo<subTwoSize)
    {
        a[indexMerged] = subTwo[indextwo];
        indextwo++;
        indexMerged++;
    }
    
    delete []subOne;
    delete []subTwo;
}
```

## 5. Quick Sort

We first choose an item in the array to be the pivot. Then put all items that are less than the pivot to the left and those that are greater to the right (secure the spot of the pivot). Recursively quick sort the both subarrays until the subarray has only one or zero element.  

**Note:** Quick Sort uses idea “it is easier to sort two smaller lists than one large one”  
**Time complexity:** O(n^2) if the array is not split evenly; O(N*logN) if it is spilt evenly  
**In-place:** yes  
**Sample process:**  
![quick sort process](/assets/images/quick-sort-process.jpg)  
**My code:**

```cpp
/*quick sort*/
void QuickSortRecursion(int arr[], int first, int last);
int partition(int arr[], int first, int last);

void QuickSort(int arr[], int size)
{
    QuickSortRecursion(arr, 0, size-1);
}

void QuickSortRecursion(int arr[], int first, int last)
{
    if (first<last)
    {
        int PivotIndex = partition(arr, first, last);
        QuickSortRecursion(arr, first, PivotIndex-1);
        QuickSortRecursion(arr, PivotIndex+1, last);
    }
}

int partition(int arr[], int first, int last)
{
    int pivot = arr[last];
    int pivotIndex = first;

    for(int i=first; i<last; i++)
    {
        if(arr[i]<=pivot)
        {
            swap(arr[i], arr[pivotIndex]);
            pivotIndex++;
        }
    }
    swap(arr[pivotIndex], arr[last]);
    return pivotIndex;
}
```

## 6. Heap Sort

Heap sort is a comparison-based sorting algorithm based on a binary heap tree. Binary heap tree is binary tree with special shape and order. Its shape is complete (full and can add one more last one with all node on the left) and the value of parent node is greater or equal to both children.

A heap can be stored in an array. We can compress a binary tree into a linear represented one-dimensional array. If the parent is in the index n, its left child will be in 2*n+1 and right child will be in 2*n+2.

Heap sort uses the characteristic of heap that the root of a heap must be the greatest value in the list. We first make the list into a heap, so the root of the heap (array[0]) will be the greatest. Swap the root with the last item in the array and decrease the size by one. Use “ReheapDown” to make the array to be a heap and repeat the process until the size of array become 0.

**Checker:** no use  
**Time complexity:** O(n*logN) [ReheapDown process is O(logN); swap and sort will be O(N)]  
**In-place:** yes  
**Sample process:**
![heao sort process](/assets/images/heap-sort-process.jpg)
**My code:**

```cpp
/*heap sort*/
void ReheapDown(int arr[], int size, int index);

void HeapSort(int arr[], int size)
{   //turn the array into a heap
    for (int i=size/2 -1; i>=0; i--)
        ReheapDown(arr, size, i);
    for (int i=size -1; i>0; i--)
    {   
        //put the largest index to the bottom of the array
        swap(arr[0], arr[i]);
        //get the largest value to be the root (a[0])
        ReheapDown(arr, i-1, 0);
    }
}

void ReheapDown(int arr[], int size, int index)
{   
    // first assume the root has largest value
    int largestIndex = index;
    int leftIndex = 2*index+1;
    int rightIndex = 2*index+2;

    if (leftIndex<size && arr[leftIndex]>arr[largestIndex])
        largestIndex = leftIndex;
    if (rightIndex<size && arr[rightIndex]>arr[largestIndex])
        largestIndex = rightIndex;
    if (largestIndex != index)
    {
        swap(arr[largestIndex], arr[index]);
        ReheapDown(arr, size, largestIndex);
    }
}
```

## 7.  Radix Sort

Radix sort is not sorting an array by comparing the values. It sorts by “classifying” the array with the radix of each digit. Radix is the number of possibilities for each position. The lowercase letters have a radix of 26; the decimal numbers have a radix of 10.

There are 2 kinds of radix sort. One of them starts at the most significant digit; the other starts at the least significant digit/ There is no big different with these two kinds: they are just different.

Here we discuss radix sort starting from the least significant digit. We have 10 queues representing digits from 1 to 9. All the items with a 0 in the ones position will be put into queue[0]. After all items are classified into the right queues based on their ones position, collect the numbers in queue back to the array and repeat classifying for the other digits.

**Checker:** no use  
**Time complexity:** O(N*P) [P: number of digits in the item] Since P is likely small relative to N, so simply O(N)  
**In-place:** no (yes if you use linked list and linked queue)  
**Sample process:**
![radix sort process](/assets/images/radix-sort-procee.jpg)
**My code:**

```cpp
/*radix sort*/
int getMax(int arr[], int size);
void countSort(int arr[], int size, int digit);

void RadixSort(int arr[], int size)
{
    int max = getMax(arr, size);

    for (int digit=1; max/digit>1; digit *= 10)
    {
        countSort(arr, size, digit);
    }

}

int getMax(int arr[], int size)
{
    int max=arr[0];
    for (int i=1; i<size; i++)
    {
        if (arr[i]>max)
            max=arr[i];
    }
    return max;
}

void countSort(int arr[], int size, int digit)
{
    int *fixed;
    fixed = new int[size];
    int counter[10]={0}; // 10 radixes
    
    for (int i=0; i<size; i++)
    {
        counter[ (arr[i]/digit) % 10]++;
    }

    for (int j=1; j<10; j++)
    {
        counter[j] += counter[j-1]; 
    }

    for (int k=size-1; k>=0; k--)
    {
        fixed[counter[(arr[k]/digit) % 10]-1] = arr[k];
        counter[ (arr[k]/digit) % 10]--;
    }

    for (int p = 0; p < size; p++)
    {
        arr[p] = fixed[p];
    }

    delete[] fixed;
}
```  
