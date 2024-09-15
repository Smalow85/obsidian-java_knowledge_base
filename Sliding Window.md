**Sliding Window Technique** is a method used to efficiently solve problems that involve defining a **window** or **range** in the input data (arrays or strings) and then moving that window across the data to perform some operation within the window. This technique is commonly used in algorithms like *finding subarrays with a specific sum, finding the longest substring with unique characters, or solving problems that require a fixed-size window to process elements efficiently*.

**Example of problem:** Calculate maximum sum of subarray of `K` elements from array of `N` elements.

Instead of taking each `K` sized subarray and calculating its sum, we can just take one `K` size subarray from 0 to K-1 index and calculate its sum now shift our range one by one along with the iterations and update the result, like in next iteration increase the left and right pointer and update the previous sum as shown in the below image:

![[Pasted image 20240728172041.png]]

There are basically two types of sliding window:

#### 1. Fixed Size Sliding Window:
The general steps to solve these questions by following below steps:
- Find the size of the window required, say K.
- Compute the result for 1st window, i.e. include the first K elements of the data structure.
- Then use a loop to slide the window by 1 and keep computing the result window by window.
#### 2. Variable Size Sliding Window:
The general steps to solve these questions by following below steps:
- In this type of sliding window problem, we increase our right pointer one by one till our condition is true.
- At any step if our condition does not match, we shrink the size of our window by increasing left pointer.
- Again, when our condition satisfies, we start increasing the right pointer and follow step 1.
- We follow these steps until we reach to the end of the array.

## How to Identify Sliding Window Problems:
- These problems generally require Finding Maximum/Minimum Subarray, Substrings which satisfy some specific condition.
- The size of the subarray or substring `K` will be given in some of the problems.
- These problems can easily be solved in O(N2) time complexity using nested loops, using sliding window we can solve these in O(n) Time Complexity.
- Required Time Complexity: O(N) or O(N log(N))
- Constraints: N <= 106 , If N is the size of the Array/String.

**Use case and code example:**

```java
class GFG {

    // Returns maximum sum in
    // a subarray of size k.
    static int maxSum(int arr[], int n, int k)
    {
        // n must be greater
        if (n <= k) {
            System.out.println("Invalid");
            return -1;
        }

        // Compute sum of first window of size k
        int max_sum = 0;
        for (int i = 0; i < k; i++)
            max_sum += arr[i];

        // Compute sums of remaining windows by
        // removing first element of previous
        // window and adding last element of
        // current window.
        int window_sum = max_sum;
        for (int i = k; i < n; i++) {
            window_sum += arr[i] - arr[i - k];
            max_sum = Math.max(max_sum, window_sum);
        }

        return max_sum;
    }
}
```


