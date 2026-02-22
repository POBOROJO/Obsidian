### 1. Intuition
"Divide and Merge."
This is a classic Divide and Conquer algorithm. You divide the array into two equal halves until each half contains only one element. A single element is always sorted. Then, you start merging these sorted halves back together, step by step, until the entire array is sorted.

### 2. Approach
1. **Divide (`mergeSort` function):** 
   - Find the middle index: `mid = (low + high) / 2`.
   - Recursively call `mergeSort` for the left half: `low` to `mid`.
   - Recursively call `mergeSort` for the right half: `mid + 1` to `high`.
   - Base case: If `low >= high`, it means the array has 1 or 0 elements, so we just return.
2. **Merge (`merge` function):**
   - Use two pointers. `left` points to `low`, and `right` points to `mid + 1`.
   - Compare `arr[left]` and `arr[right]`. Add the smaller one to a temporary list and move the respective pointer.
   - If one of the halves gets exhausted, copy the remaining elements of the other half into the temporary list.
   - Finally, copy the elements from the temporary list back to the original array.

### 3. Java Code

*Java Note for C++ transition:*
- In C++, you would use `std::vector<int> temp;`. In Java, we use `ArrayList<Integer> temp = new ArrayList<>();`. 
- To add elements, use `temp.add(val)`. To get elements, use `temp.get(index)`.

```java
import java.util.*;

public class Main {
    
    public static void merge(int[] arr, int low, int mid, int high) {
        ArrayList<Integer> temp = new ArrayList<>(); // temporary array
        int left = low;      // starting index of left half
        int right = mid + 1; // starting index of right half

        // Step 1: Compare and add smaller elements to temp
        while (left <= mid && right <= high) {
            if (arr[left] <= arr[right]) {
                temp.add(arr[left]);
                left++;
            } else {
                temp.add(arr[right]);
                right++;
            }
        }

        // Step 2: If elements on the left half are still left
        while (left <= mid) {
            temp.add(arr[left]);
            left++;
        }

        // Step 3: If elements on the right half are still left
        while (right <= high) {
            temp.add(arr[right]);
            right++;
        }

        // Step 4: Transfer all elements from temp back to original array
        for (int i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    public static void mergeSort(int[] arr, int low, int high) {
        if (low >= high) return; // Base case
        
        int mid = (low + high) / 2;
        
        mergeSort(arr, low, mid);      // Sort left half
        mergeSort(arr, mid + 1, high); // Sort right half
        merge(arr, low, mid, high);    // Merge them
    }

    public static void main(String[] args) {
        // Input
        int[] arr = {3, 2, 4, 1, 3};
        int n = arr.length;
        
        System.out.println("Input: " + Arrays.toString(arr));
        
        mergeSort(arr, 0, n - 1);
        
        // Output
        System.out.println("Output: " + Arrays.toString(arr));
    }
}
```

### 4. Complexity & Edge Cases
- **Time Complexity:** $O(N \log_2 N)$ for Best, Worst, and Average cases. 
  - We divide the array in half $\log_2 N$ times.
  - At each step of division, we merge the elements, which takes $O(N)$ time.
- **Space Complexity:** $O(N)$ because we use a temporary `ArrayList` to store elements during the merge process.
- **Edge Cases:**
  - Array of size 1 (Base case `low >= high` handles it).
  - Array with duplicate elements (Handled correctly. Using `<=` in `arr[left] <= arr[right]` ensures Merge Sort is **stable**).

---
