### 1. Intuition
"Select the minimum and swap." 
You divide the array into two parts: sorted and unsorted. You repeatedly find the minimum element from the unsorted part and put it at the beginning of the unsorted part.

### 2. Approach (Optimal for Selection Sort)
1. Run an outer loop `i` from `0` to `n-2`. (The last element will automatically be sorted).
2. Assume the current `i` is the minimum.
3. Run an inner loop `j` from `i` to `n-1` to find the actual minimum.
4. Swap the element at `i` with the actual minimum.

### 3. Java Code

*Java Note for C++ transition:* 
- Arrays are declared as `int[] arr` instead of `int arr[]` (though both work, the former is standard Java). 
- To get the size of an array, use `arr.length` (no need for `sizeof(arr)/sizeof(arr[0])`).

```java
import java.util.*;

public class Main {
    
    public static void selectionSort(int[] arr, int n) {
        // Step 1: i goes up to n-2
        for (int i = 0; i <= n - 2; i++) {
            int minIndex = i;
            
            // Step 2 & 3: Find the minimum in the unsorted array
            for (int j = i; j <= n - 1; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            
            // Step 4: Swap arr[i] and arr[minIndex]
            int temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }

    public static void main(String[] args) {
        // Input
        int[] arr = {13, 46, 24, 52, 20, 9};
        int n = arr.length;
        
        System.out.println("Input: " + Arrays.toString(arr));
        
        selectionSort(arr, n);
        
        // Output
        System.out.println("Output: " + Arrays.toString(arr));
    }
}
```

### 4. Complexity & Edge Cases
- **Time Complexity:** $O(N^2)$ for Best, Worst, and Average cases. The inner loop always runs regardless of whether the array is sorted or not.
- **Space Complexity:** $O(1)$ because we are modifying the array in-place. No extra space used.
- **Edge Cases:** 
  - Array of size 1 (loop condition `i <= n-2` handles it, loop won't run).
  - Array with duplicate elements (handled correctly, relative order might change, meaning Selection Sort is **unstable**).

---
