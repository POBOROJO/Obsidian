### 1. Intuition
"Push the maximum to the last by adjacent swaps."
You repeatedly compare adjacent elements. If the left element is greater than the right element, you swap them. After one full pass, the largest element "bubbles up" to the last position. You repeat this for the remaining unsorted portion.

### 2. Approach (Brute to Optimal)
**Brute Force:**
1. Run an outer loop `i` from `n-1` down to `1`. (This represents the boundary of the unsorted array).
2. Run an inner loop `j` from `0` to `i-1`.
3. If `arr[j] > arr[j+1]`, swap them. 

**Optimal (The Optimization):**
What if the array is already sorted? The brute force will still run $O(N^2)$ times. 
We introduce a `didSwap` flag. If no swaps occur during an entire pass of the inner loop, it means the array is perfectly sorted. We can `break` out of the outer loop immediately.

### 3. Java Code

*Java Note for C++ transition:* 
- Java doesn't have pointers, so you can't pass variables by reference to a `swap()` function easily like `swap(&a, &b)` in C++. We just do the swap inline using a `temp` variable.

```java
import java.util.*;

public class Main {
    
    public static void bubbleSort(int[] arr, int n) {
        // Step 1: i goes from n-1 down to 1
        for (int i = n - 1; i >= 1; i--) {
            int didSwap = 0; // Optimization flag
            
            // Step 2: Push the max to the end of the current unsorted portion
            for (int j = 0; j <= i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Step 3: Swap adjacent elements
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    
                    didSwap = 1;
                }
            }
            
            // If no swaps happened, array is sorted. Break early.
            if (didSwap == 0) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        // Input
        int[] arr = {13, 46, 24, 52, 20, 9};
        int n = arr.length;
        
        System.out.println("Input: " + Arrays.toString(arr));
        
        bubbleSort(arr, n);
        
        // Output
        System.out.println("Output: " + Arrays.toString(arr));
    }
}
```

### 4. Complexity & Edge Cases
- **Time Complexity:** 
  - **Worst & Average Case:** $O(N^2)$ (Array is reverse sorted or jumbled).
  - **Best Case:** $O(N)$ (Array is already sorted, inner loop runs once, `didSwap` remains 0, and we break).
- **Space Complexity:** $O(1)$ (In-place sorting).
- **Edge Cases:**
  - Already sorted array (Handled in $O(N)$ time due to `didSwap`).
  - Array with duplicate elements (Handled correctly, Bubble Sort is **stable** because we only swap if strictly greater `>`).

---