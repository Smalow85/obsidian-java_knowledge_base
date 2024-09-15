## Sorting array
### Characteristics of Sorting Algorithms:

- **Time Complexity:** Time complexity, a measure of how long it takes to run an algorithm, is used to categorize sorting algorithms. The worst-case, average-case, and best-case performance of a sorting algorithm can be used to quantify the time complexity of the process.
- **Space Complexity:** Sorting algorithms also have space complexity, which is the amount of memory required to execute the algorithm.
- **Stability:** A sorting algorithm is said to be stable if the relative order of equal elements is preserved after sorting. This is important in certain applications where the original order of equal elements must be maintained.
- **In-Place Sorting:** An in-place sorting algorithm is one that does not require additional memory to sort the data. This is important when the available memory is limited or when the data cannot be moved.
- **Adaptivity:** An adaptive sorting algorithm is one that takes advantage of pre-existing order in the data to improve performance.

## Sorting Algorithms

### Selection Sort

**Selection sort** is a simple and efficient sorting algorithm that works by repeatedly selecting the smallest (or largest) element from the unsorted portion of the list and moving it to the sorted portion of the list.

Array example:  {64, 25, 12, 22, 11}

1. ![[Pasted image 20240727235444.png]]
2. ![[Pasted image 20240727235458.png]]
3. ![[Pasted image 20240727235516.png]]
4. ![[Pasted image 20240727235526.png]]
**Code example:**

```java
public class SelectionSort
{
    void sort(int arr[])
    {
        int n = arr.length;

        // One by one move boundary of unsorted subarray
        for (int i = 0; i < n-1; i++)
        {
            // Find the minimum element in unsorted array
            int min_idx = i;
            for (int j = i+1; j < n; j++)
                if (arr[j] < arr[min_idx])
                    min_idx = j;

            // Swap the found minimum element with the first
            // element
            int temp = arr[min_idx];
            arr[min_idx] = arr[i];
            arr[i] = temp;
        }
    }
}
```

**Time Complexity:** The time complexity of Selection Sort is O(N2) as there are two nested loops
**Auxiliary Space:** O(1)

### Bubble Sort

In Bubble Sort algorithm, 

- traverse from left and compare adjacent elements and the higher one is placed at right side. 
- In this way, the largest element is moved to the rightmost end at first. 
- This process is then continued to find the second largest and place it and so on until the data is sorted.

Array example {6, 0, 3, 5}

1. ![[Pasted image 20240728000516.png]]
2. ![[Pasted image 20240728000533.png]]
3. ![[Pasted image 20240728000608.png]]
**Code example:**

```java
class GFG {
    
    // An optimized version of Bubble Sort
    static void bubbleSort(int arr[], int n)
    {
        int i, j, temp;
        boolean swapped;
        for (i = 0; i < n - 1; i++) {
            swapped = false;
            for (j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap arr[j] and arr[j+1]
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }

            // If no two elements were
            // swapped by inner loop, then break
            if (swapped == false)
                break;
        }
    }
}
```

**Time Complexity:** O(N2)  
**Auxiliary Space:** O(1)

### Insertion Sort

To achieve insertion sort, follow these steps:

- We have to start with second element of the array as first element in the array is assumed to be sorted.
- Compare second element with the first element and check if the second element is smaller then swap them.
- Move to the third element and compare it with the second element, then the first element and swap as necessary to put it in the correct position among the first three elements.
- Continue this process, comparing each element with the ones before it and swapping as needed to place it in the correct position among the sorted elements.
- Repeat until the entire array is sorted.

![[Pasted image 20240728001602.png]]

**Code example:**

```java
public class InsertionSort {
    /*Function to sort array using insertion sort*/
    void sort(int arr[])
    {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;

            /* Move elements of arr[0..i-1], that are
               greater than key, to one position ahead
               of their current position */
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            arr[j + 1] = key;
        }
    }
};
```

**Time Complexity:** O(N^2)  
**Auxiliary Space:** O(1)

### Merge Sort

Here’s a step-by-step explanation of how merge sort works:

1. **Divide:** Divide the list or array recursively into two halves until it can no more be divided.
2. **Conquer:** Each subarray is sorted individually using the merge sort algorithm.
3. **Merge:** The sorted subarrays are merged back together in sorted order. The process continues until all elements from both subarrays have been merged.

1. ![[Pasted image 20240728004633.png]]
2. ![[Pasted image 20240728004643.png]]
3. ![[Pasted image 20240728004659.png]]
4. ![[Pasted image 20240728004709.png]]
**Code example:**

```java
class MergeSort {

    // Merges two subarrays of arr[].
    // First subarray is arr[l..m]
    // Second subarray is arr[m+1..r]
    void merge(int arr[], int l, int m, int r)
    {
        // Find sizes of two subarrays to be merged
        int n1 = m - l + 1;
        int n2 = r - m;

        // Create temp arrays
        int L[] = new int[n1];
        int R[] = new int[n2];

        // Copy data to temp arrays
        for (int i = 0; i < n1; ++i)
            L[i] = arr[l + i];
        for (int j = 0; j < n2; ++j)
            R[j] = arr[m + 1 + j];

        // Merge the temp arrays

        // Initial indices of first and second subarrays
        int i = 0, j = 0;

        // Initial index of merged subarray array
        int k = l;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k] = L[i];
                i++;
            }
            else {
                arr[k] = R[j];
                j++;
            }
            k++;
        }

        // Copy remaining elements of L[] if any
        while (i < n1) {
            arr[k] = L[i];
            i++;
            k++;
        }

        // Copy remaining elements of R[] if any
        while (j < n2) {
            arr[k] = R[j];
            j++;
            k++;
        }
    }

    // Main function that sorts arr[l..r] using
    // merge()
    void sort(int arr[], int l, int r)
    {
        if (l < r) {

            // Find the middle point
            int m = l + (r - l) / 2;

            // Sort first and second halves
            sort(arr, l, m);
            sort(arr, m + 1, r);

            // Merge the sorted halves
            merge(arr, l, m, r);
        }
    }
}
```

**Time Complexity:**

- **Best Case:** O(n log n), When the array is already sorted or nearly sorted.
- **Average Case:** O(n log n), When the array is randomly ordered.
- **Worst Case:** O(n log n), When the array is sorted in reverse order.

**Space Complexity:** O(n), Additional space is required for the temporary array used during merging.

### Quick Sort

**QuickSort** is a sorting algorithm based on the [Divide and Conquer algorithm](https://www.geeksforgeeks.org/divide-and-conquer-algorithm-introduction/) that picks an element as a pivot and partitions the given array around the picked pivot by placing the pivot in its correct position in the sorted array.

The key process in **quickSort** is a **partition()** . The target of partitions is to place the pivot (any element can be chosen to be a pivot) at its correct position in the sorted array and put all smaller elements to the left of the pivot, and all greater elements to the right of the pivot.

Partition is done recursively on each side of the pivot after the pivot is placed in its correct position and this finally sorts the array.

array = {10, 80, 30, 90, 40}

1. ![[Pasted image 20240728145349.png]]
2. ![[Pasted image 20240728145400.png]]
3. ![[Pasted image 20240728145412.png]]
4. ![[Pasted image 20240728145428.png]]
5. ![[Pasted image 20240728145437.png]]
**Code example:**

```java
class GFG {

    // A utility function to swap two elements
    static void swap(int[] arr, int i, int j)
    {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    // This function takes last element as pivot,
    // places the pivot element at its correct position
    // in sorted array, and places all smaller to left
    // of pivot and all greater elements to right of pivot
    static int partition(int[] arr, int low, int high)
    {
        // Choosing the pivot
        int pivot = arr[high];

        // Index of smaller element and indicates
        // the right position of pivot found so far
        int i = (low - 1);

        for (int j = low; j <= high - 1; j++) {

            // If current element is smaller than the pivot
            if (arr[j] < pivot) {

                // Increment index of smaller element
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, i + 1, high);
        return (i + 1);
    }

    // The main function that implements QuickSort
    // arr[] --> Array to be sorted,
    // low --> Starting index,
    // high --> Ending index
    static void quickSort(int[] arr, int low, int high)
    {
        if (low < high) {

            // pi is partitioning index, arr[p]
            // is now at right place
            int pi = partition(arr, low, high);

            // Separately sort elements before
            // partition and after partition
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }

	public static void main(String[] args)
    {
        int[] arr = { 10, 7, 8, 9, 1, 5 };
        int N = arr.length;

        // Function call
        quickSort(arr, 0, N - 1);
        System.out.println("Sorted array:");
        printArr(arr);
    }
}
```

**Time Complexity:**

- **Best Case** : Ω (N log (N))  
    The best-case scenario for quicksort occur when the pivot chosen at the each step divides the array into roughly equal halves.  
    In this case, the algorithm will make balanced partitions, leading to efficient Sorting.
- **Average Case:** θ ( N log (N))  
    Quicksort’s average-case performance is usually very good in practice, making it one of the fastest sorting Algorithm.
- **Worst Case:** O(N ^ 2)  
    The worst-case Scenario for Quicksort occur when the pivot at each step consistently results in highly unbalanced partitions. When the array is already sorted and the pivot is always chosen as the smallest or largest element. To mitigate the worst-case Scenario, various techniques are used such as choosing a good pivot (e.g., median of three) and using Randomized algorithm (Randomized Quicksort ) to shuffle the element before sorting.
- **Auxiliary Space:** O(1), if we don’t consider the recursive stack space. If we consider the recursive stack space then, in the worst case quicksort could make __O__ ( __N__ ).


