# [Leetcode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Using a Dictionary](#approach-2-optimized-using-a-dictionary)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves checking each pair to see if it is a good pair. A good pair is defined as `nums[i] == nums[j]` and `i < j`. This approach simply uses two nested loops to explore all possible pairs.

### Algorithm
1. Initialize a `count` variable to 0, which will store the number of good pairs.
2. Iterate through each element in the array using index `i`.
3. For each index `i`, iterate through the array starting from index `i+1` using index `j`.
4. Check if `nums[i] == nums[j]`, and if so, increment the `count`.
5. Return the `count` after checking all possible pairs.

### Code
```csharp
public class Solution {
    public int NumIdenticalPairs(int[] nums) {
        int count = 0;
        
        // Iterate over all elements with a nested loop
        for (int i = 0; i < nums.Length; i++) {
            for (int j = i + 1; j < nums.Length; j++) {
                // Check if the pair (i, j) is a good pair
                if (nums[i] == nums[j]) {
                    count++;
                }
            }
        }
        
        return count;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the number of elements in the array. This is because for each element, we are potentially iterating through almost all other elements.
- **Space Complexity**: O(1), since we are only using a constant amount of extra space.

---

## Approach 2: Optimized Using a Dictionary

### Intuition
The brute force approach can be improved by using a dictionary to track the occurrences of each number in the array. This can help us quickly calculate the number of good pairs because for each duplicate value, the number of possible pairs increases cumulatively.

### Algorithm
1. Initialize a dictionary `countDict` to store the count of each unique number in the array.
2. Initialize a `count` variable to zero.
3. Iterate through the array, for each element:
   - If the element is already in `countDict`, it means there are `countDict[element]` good pairs possible with the current element. Add this to `count`.
   - Increment the count of the current element in `countDict`.
4. Return the `count` after processing all elements.

### Code
```csharp
using System.Collections.Generic;

public class Solution {
    public int NumIdenticalPairs(int[] nums) {
        int count = 0;
        Dictionary<int, int> countDict = new Dictionary<int, int>();
        
        foreach (int num in nums) {
            // If the number exists in the dictionary, add its current count to the total count
            if (countDict.ContainsKey(num)){
                count += countDict[num];
                countDict[num]++;
            } else {
                // Otherwise, initialize it in the dictionary
                countDict[num] = 1;
            }
        }
        
        return count;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. We iterate over the array once and perform O(1) operations for each element using the dictionary.
- **Space Complexity**: O(n), in the worst case scenario where all elements are unique and hence the dictionary could contain all elements.

