## Searching terminologies:
### Target Element:
In searching, there is always a specific target element or item that you want to find within the data collection. This target could be a value, a record, a key, or any other data entity of interest.
### Search Space:
The search space refers to the entire collection of data within which you are looking for the target element. Depending on the data structure used, the search space may vary in size and organization.
### Complexity:
Searching can have different levels of complexity depending on the data structure and the algorithm used. The complexity is often measured in terms of time and space requirements.
### Deterministic vs. Non-deterministic:
Some searching algorithms, like **binary search**, are deterministic, meaning they follow a clear, systematic approach. Others, such as linear search, are non-deterministic, as they may need to examine the entire search space in the worst case.

**Binary Search Algorithm** is a [searching algorithm](https://www.geeksforgeeks.org/searching-algorithms/) used in a sorted array by **repeatedly dividing the search interval in half**. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to `O(log N)`.
## Conditions:
- The data structure must be sorted.
- Access to any element of the data structure should take constant time.
## How does Binary Search Algorithm work?
Consider an array = {2, 5, 8, 12, 16, 23, 38, 56, 72, 91}, and the target = 23

1. ![[Pasted image 20240728164811.png]]
2. ![[Pasted image 20240728164820.png]]
3. ![[Pasted image 20240728164828.png]]
4. ![[Pasted image 20240728164836.png]]

## How to Implement Binary Search Algorithm?

The **Binary Search Algorithm** can be implemented in the following two ways

- Iterative Binary Search Algorithm
- Recursive Binary Search Algorithm

### Iterative Binary Search Algorithm:
Here we use a while loop to continue the process of comparing the key and splitting the search space in two halves.

```java
class BinarySearch {
  
    // Returns index of x if it is present in arr[].
    int binarySearch(int arr[], int x)
    {
        int low = 0, high = arr.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Check if x is present at mid
            if (arr[mid] == x)
                return mid;

            // If x greater, ignore left half
            if (arr[mid] < x)
                low = mid + 1;

            // If x is smaller, ignore right half
            else
                high = mid - 1;
        }
        // If we reach here, then element was
        // not present
        return -1;
    }
}
```

### Recursive Binary Search Algorithm:

Create a recursive function and compare the mid of the search space with the key. And based on the result either return the index where the key is found or call the recursive function for the next search space.

```java
class BinarySearch {

    // Returns index of x if it is present in arr[low..
    // high], else return -1
    int binarySearch(int arr[], int low, int high, int x)
    {
        if (high >= low) {
            int mid = low + (high - low) / 2;

            // If the element is present at the
            // middle itself
            if (arr[mid] == x)
                return mid;

            // If element is smaller than mid, then
            // it can only be present in left subarray
            if (arr[mid] > x)
                return binarySearch(arr, low, mid - 1, x);

            // Else the element can only be present
            // in right subarray
            return binarySearch(arr, mid + 1, high, x);
        }

        // We reach here when element is not present
        // in array
        return -1;
    }
}
```

- **Time Complexity:**
    - Best Case: O(1)
    - Average Case: O(log N)
    - Worst Case: O(log N)
- **Auxiliary Space:** O(1), If the recursive call stack is considered then the auxiliary space will be O(logN).
