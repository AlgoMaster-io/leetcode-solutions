# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approach 1: Using Extra Array

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public void Rotate(int[] nums, int k) {
        int n = nums.Length;
        k = k % n; // Handle cases where k > n
        int[] rotated = new int[n];

        // Copy rotated elements to a new array
        for (int i = 0; i < n; i++) {
            rotated[(i + k) % n] = nums[i];
        }

        // Copy elements back to the original array
        for (int i = 0; i < n; i++) {
            nums[i] = rotated[i];
        }
    }
}
```

## Approach 2: Rotate One-by-One (Brute Force)

### Solution
csharp
```csharp
// Time Complexity: O(n * k)
// Space Complexity: O(1)
public class Solution {
    public void Rotate(int[] nums, int k) {
        int n = nums.Length;
        k = k % n; // Handle cases where k > n

        // Rotate the array k times
        for (int i = 0; i < k; i++) {
            int last = nums[n - 1]; // Save the last element
            // Shift elements to the right
            for (int j = n - 1; j > 0; j--) {
                nums[j] = nums[j - 1];
            }
            nums[0] = last; // Place the saved element at the beginning
        }
    }
}
```

## Approach 3: Reverse Array (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void Rotate(int[] nums, int k) {
        int n = nums.Length;
        k = k % n; // Handle cases where k > n

        // Step 1: Reverse the entire array
        Reverse(nums, 0, n - 1);

        // Step 2: Reverse the first k elements
        Reverse(nums, 0, k - 1);

        // Step 3: Reverse the remaining n - k elements
        Reverse(nums, k, n - 1);
    }

    private void Reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

## Approach 4: Cyclic Replacements

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void Rotate(int[] nums, int k) {
        int n = nums.Length;
        k = k % n; // Handle cases where k > n
        int count = 0; // Number of elements rotated

        for (int start = 0; count < n; start++) {
            int current = start;
            int prev = nums[start];

            do {
                int next = (current + k) % n;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
                count++;
            } while (start != current); // Cycle ends when we return to the start
        }
    }
}
```

