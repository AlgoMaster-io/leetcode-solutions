# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches:
- [Approach 1: Basic Brute Force](#approach-1-basic-brute-force)
- [Approach 2: Sorting](#approach-2-sorting)
- [Approach 3: Hash Set](#approach-3-hash-set)
- [Approach 4: In-place Hashing (Optimal)](#approach-4-in-place-hashing)

### Approach 1: Basic Brute Force

#### Intuition:
The simplest approach is to check each positive integer from 1 onwards to see if it's missing. This involves scanning through the array repeatedly until the smallest missing positive number is found.

```csharp
public class Solution {
    public int FirstMissingPositive(int[] nums) {
        int num = 1;
        while (true) {
            // Check if `num` is present in the array
            bool found = false;
            foreach (int x in nums) {
                if (x == num) {
                    found = true;
                    break;
                }
            }
            // If `num` is not found, return it as the smallest missing positive
            if (!found) return num;
            num++;
        }
    }
}
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

---

### Approach 2: Sorting

#### Intuition:
By sorting the array, the numbers are rearranged in increasing order, and we can then scan through the sorted array to find the first missing positive integer. This approach leverages sorting, which ensures that any missing numbers are easily identifiable.

```csharp
public class Solution {
    public int FirstMissingPositive(int[] nums) {
        // Sort the input array
        Array.Sort(nums);
        int num = 1;
        foreach (int x in nums) {
            // Ignore non-positive numbers
            if (x <= 0) continue;
            // Check if current number matches the expected positive number
            if (x == num) num++;
            else if (x > num) break;  // Since sorted, we missed `num`
        }
        // Return the first missing positive number
        return num;
    }
}
```

**Time Complexity:** O(n log n)  
**Space Complexity:** O(1) (ignoring space used by sorting)

---

### Approach 3: Hash Set

#### Intuition:
Using a hash set allows us to track the presence of each number efficiently. We iterate over the numbers and add them to a hash set, then we repeatedly check for the first missing positive integer starting from 1.

```csharp
public class Solution {
    public int FirstMissingPositive(int[] nums) {
        HashSet<int> numSet = new HashSet<int>(nums);
        int num = 1;
        while (numSet.Contains(num)) {
            // Increment the number until a missing one is found
            num++;
        }
        return num;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

### Approach 4: In-place Hashing (Optimal)

#### Intuition:
We can use the array itself to track the presence of numbers by reordering the elements to put each number in its correct position. Specifically, we rearrange the array such that if a number is `k`, it should ideally be placed at index `k-1`. This doesn't require extra space.

```csharp
public class Solution {
    public int FirstMissingPositive(int[] nums) {
        int n = nums.Length;
        
        for (int i = 0; i < n; i++) {
            // Place nums[i] in its correct position if it is in the range [1, n]
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                // Swap values to their correct positions
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }

        // Identify the first index not satisfying nums[i] == i+1
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        
        // If all positions are correct, the answer is n + 1
        return n + 1;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

This final approach is optimal because it uses a constant amount of extra space and traverses the array a minimal number of times. By rearranging the array elements within the array itself, it efficiently determines the first missing positive integer.

