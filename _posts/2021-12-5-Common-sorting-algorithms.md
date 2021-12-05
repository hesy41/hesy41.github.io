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

1. straight selection sort
2. bubble sort
3. insertion sort
4. merge sort
5. quick sort
6. heap sort
7. radix sort

## 1. Straight Selection Sort

We first tag the first index in the array with “current position”. Then find the smallest element from “current position” to the last element in the array. Swap the “current position” with the smallest element. Move the “current position” tag to the next element. When the “current position” tag is passed to the last element in the array, the array is sorted.  

**Checker:** no use  
**Time complexity:** O(n^2)  
**In-place:** Yes  
**Sample process:**  
![Straight Selection Sort process](/assets/images/Straight-Selection-Sort-process.jpg)
**My code:**  
```  
/*Straight Selection Sort*/
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
