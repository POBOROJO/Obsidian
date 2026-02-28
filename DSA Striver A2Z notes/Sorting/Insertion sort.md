### 1. Intuition
"Take an element and place it in its correct order."
Think of how you sort playing cards in your hand. You pick up a card, look at the cards you already hold, and insert the new card exactly where it belongs by shifting the larger cards to the right.

### 2. Approach
1. Run an outer loop `i` from `0` to `n-1`. This picks the current element you want to place.
2. Run an inner `while` loop with a pointer `j` starting at `i`.
3. Keep comparing `arr[j]` with its left neighbor `arr[j-1]`.
4. If the left neighbor is greater, swap them and move `j` one step to the left (`j--`).
5. Stop when the left neighbor is smaller or equal, or when `j` reaches `0` (meaning the element is now at the very front).

### 3. Java Code

*Java Note for C++ transition:*
- In C++, you might sometimes write `while(j > 0 && arr[j-1] > arr[j])`. In Java, this is exactly the same, but remember that Java evaluates boolean expressions strictly left-to-right (short-circuiting). Always put the bounds check (`j > 0`) first to avoid `ArrayIndexOutOfBoundsException`.

```java
import java.util.*;

public class Main {
    
    public static void insertionSort(int[] arr, int n) {
        // Step 1: i goes from 0 to n-1
        for (int i = 0; i <= n - 1; i++) {
            int j = i;
            
            // Step 2, 3 & 4: Swap leftwards until the correct position is found
            while (j > 0 && arr[j - 1] > arr[j]) {
                int temp = arr[j - 1];
                arr[j - 1] = arr[j];
                arr[j] = temp;
                
                j--;
            }
        }
    }

    public static void main(String[] args) {
        // Input
        int[] arr = {13, 46, 24, 52, 20, 9};
        int n = arr.length;
        
        System.out.println("Input: " + Arrays.toString(arr));
        
        insertionSort(arr, n);
        
        // Output
        System.out.println("Output: " + Arrays.toString(arr));
    }
}
```

### 4. Complexity & Edge Cases
- **Time Complexity:** 
  - **Worst & Average Case:** $O(N^2)$ (Array is reverse sorted. Every element has to be swapped all the way to the 0th index).
  - **Best Case:** $O(N)$ (Array is already sorted. The `while` loop condition `arr[j - 1] > arr[j]` fails immediately at every step, so the inner loop never runs).
- **Space Complexity:** $O(1)$ (In-place sorting).
- **Edge Cases:**
  - Already sorted array (Handled optimally in $O(N)$).
  - Array with duplicates (Handled correctly, Insertion Sort is **stable** because we only swap if strictly greater `>`).

---