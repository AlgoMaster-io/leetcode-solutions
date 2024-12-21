## [Leetcode 136: Single Number](https://leetcode.com/problems/single-number/)

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using Sorting](#using-sorting)
3. [Using HashSet](#using-hashset)
4. [Optimal Approach using XOR](#optimal-approach-using-xor)

---

### Brute Force Approach

#### Intuition
In this approach, we will use the brute force method where for each element in the array, we check if it appears more than once. This can be done by iterating through the array for each element and counting its occurrences.

#### Implementation
```csharp
public class Solution {
    public int SingleNumber(int[] nums) {
        // Iterate over each element in the array
        for (int i = 0; i < nums.Length; i++) {
            int count = 0;
            // Count occurrences of nums[i] in the array
            for (int j = 0; j < nums.Length; j++) {
                if (nums[i] == nums[j]) {
                    count++;
                }
            }
            // If count is 1, we found our single number
            if (count == 1) {
                return nums[i];
            }
        }
        return -1; // Should never be hit as per problem constraints
    }
}
```

#### Time Complexity
- **O(n^2)**, where n is the number of elements in the array.
- For each element, we scan the entire array, leading to a quadratic time complexity.

#### Space Complexity
- **O(1)**, as no extra space is used except for variables.

---

### Using Sorting

#### Intuition
By sorting the array, elements with duplicates will become adjacent. Thus, the single number will be the one not having duplicates on either side.

#### Implementation
```csharp
using System;

public class Solution {
    public int SingleNumber(int[] nums) {
        Array.Sort(nums); // Sort the array
        
        // Traverse the sorted array
        for(int i = 0; i < nums.Length - 1; i += 2) {
            // If the current element is different from the next, it's the single number
            if (nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }
        
        // If not found in the loop, the single number would be the last element
        return nums[nums.Length - 1];
    }
}
```

#### Time Complexity
- **O(n log n)** due to sorting.

#### Space Complexity
- **O(1)**, as sorting is done in place and no additional space is used.

---

### Using HashSet

#### Intuition
We can use a HashSet to keep track of elements we've seen. If an element is seen for the second time, it means it’s a duplicate, and we remove it. The remaining element in the set will be the single number.

#### Implementation
```csharp
using System.Collections.Generic;

public class Solution {
    public int SingleNumber(int[] nums) {
        HashSet<int> set = new HashSet<int>();
        
        foreach (int num in nums) {
            if (set.Contains(num)) {
                set.Remove(num); // Remove if it’s a duplicate
            } else {
                set.Add(num); // Add if it's seen for the first time
            }
        }
        
        // The only element left in the set is our single number
        foreach (int num in set) {
            return num;
        }
        
        return -1; // Should never be hit as per problem constraints
    }
}
```

#### Time Complexity
- **O(n)**, where n is the number of elements in the array.

#### Space Complexity
- **O(n)**, due to the space used by the HashSet.

---

### Optimal Approach using XOR

#### Intuition
The XOR operation has a property that a ^ a = 0 and a ^ 0 = a. Therefore, XOR-ing all numbers results in canceling out all duplicate numbers and leaves the single number.

#### Implementation
```csharp
public class Solution {
    public int SingleNumber(int[] nums) {
        int result = 0; // Initialize result as 0
        
        foreach (int num in nums) {
            result ^= num; // XOR each element with the result
        }
        
        return result; // The result is the single number
    }
}
```

#### Time Complexity
- **O(n)**, where n is the number of elements in the array.

#### Space Complexity
- **O(1)**, as no extra space is used.

In the above solution, we take advantage of the XOR property to achieve both time and space optimizations. This is the most optimal approach to solve the problem.

