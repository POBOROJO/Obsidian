### 1. Intuition
"Pick a pivot and place it in its correct place."
Unlike Merge Sort which divides blindly, Quick Sort divides based on a condition. You pick any element as a pivot (let's say the first element). You figure out exactly where this pivot belongs in the final sorted array. You place it there, ensuring all elements to its left are smaller, and all elements to its right are larger. Then, you recursively do this for the left and right portions.

### 2. Approach
1. **Partition (`partition` function):**
   - Choose the first element as the `pivot`.
   - Use two pointers: `i` starting at `low`, and `j` starting at `high`.
   - Move `i` forward (`i++`) until you find an element **greater** than the pivot.
   - Move `j` backward (`j--`) until you find an element **smaller than or equal** to the pivot.
   - If `i < j`, swap `arr[i]` and `arr[j]`.
   - Repeat this until `i` crosses `j` (`i >= j`).
   - Finally, swap the `pivot` (at index `low`) with `arr[j]`. Now the pivot is in its correct sorted position. Return `j` as the partition index.
2. **Recursive Sort (`quickSort` function):**
   - Call `partition` to get the partition index `pIndex`.
   - Recursively call `quickSort` for the left part: `low` to `pIndex - 1`.
   - Recursively call `quickSort` for the right part: `pIndex + 1` to `high`.

### 3. Java Code

*Java Note for C++ transition:*
- Pay close attention to the boundary checks in the `while` loops (`i <= high - 1` and `j >= low + 1`). In Java, out-of-bounds exceptions are strictly thrown, unlike C++ which might just read garbage memory.

```java
import java.util.*;

public class Main {
    
    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[low];
        int i = low;
        int j = high;

        while (i < j) {
            // Find the first element greater than pivot
            while (arr[i] <= pivot && i <= high - 1) {
                i++;
            }
            // Find the first element smaller than or equal to pivot
            while (arr[j] > pivot && j >= low + 1) {
                j--;
            }
            // Swap if they haven't crossed
            if (i < j) {
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        
        // Place the pivot in its correct position
        int temp = arr[low];
        arr[low] = arr[j];
        arr[j] = temp;
        
        return j; // Return the partition index
    }

    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pIndex = partition(arr, low, high);
            quickSort(arr, low, pIndex - 1);  // Sort left partition
            quickSort(arr, pIndex + 1, high); // Sort right partition
        }
    }

    public static void main(String[] args) {
        // Input
        int[] arr = {4, 6, 2, 5, 7, 9, 1, 3};
        int n = arr.length;
        
        System.out.println("Input: " + Arrays.toString(arr));
        
        quickSort(arr, 0, n - 1);
        
        // Output
        System.out.println("Output: " + Arrays.toString(arr));
    }
}
```

### 4. Complexity & Edge Cases
- **Time Complexity:** 
  - **Best & Average Case:** $O(N \log N)$. The array is divided roughly into halves each time.
  - **Worst Case:** $O(N^2)$. This happens if the array is already sorted (or reverse sorted) and we pick the first element as the pivot. The partition will just separate 1 element and $N-1$ elements every time.
- **Space Complexity:** $O(1)$ auxiliary space. However, the recursive stack space takes $O(\log N)$ on average, and $O(N)$ in the worst case.
- **Edge Cases:**
  - Already sorted array (Triggers worst-case $O(N^2)$ time unless you randomize the pivot).
  - Array with all identical elements (Handled correctly, pointers will cross, but can degrade to $O(N^2)$ depending on implementation specifics. Quick Sort is **unstable**).

---
