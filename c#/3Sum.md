# [3Sum](https://leetcode.com/problems/3sum/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Sorting and Two-Pointer Approach](#sorting-and-two-pointer-approach)

### Brute Force Approach
The brute force approach for solving the 3Sum problem is to generate all possible triplets (i, j, k) where i < j < k. We then check if the sum of the elements at these indices is zero.

**Intuition:**
- Iterate through all unique pairs of indices (i, j, k) in the array, and check if their sum equals zero.
- Since we need combinations without repetitions, ensure i < j < k.

```csharp
public class Solution {
    public IList<IList<int>> ThreeSum(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();

        // Iterate through all elements for the first number
        for (int i = 0; i < nums.Length - 2; i++) {
            // Iterate for the second number
            for (int j = i + 1; j < nums.Length - 1; j++) {
                // Iterate for the third number
                for (int k = j + 1; k < nums.Length; k++) {
                    // Check if these three add up to zero
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<int> triplet = new List<int> { nums[i], nums[j], nums[k] };
                        triplet.Sort(); // Sort to ensure the triplet is in order
                        if (!ContainsTriplet(result, triplet)) {
                            result.Add(triplet);
                        }
                    }
                }
            }
        }
        
        return result;
    }

    private bool ContainsTriplet(IList<IList<int>> list, List<int> triplet) {
        foreach (var item in list) {
            if (item[0] == triplet[0] && item[1] == triplet[1] && item[2] == triplet[2]) {
                return true;
            }
        }
        return false;
    }
}
```

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n), storing the result.

### Sorting and Two-Pointer Approach
This approach reduces the time complexity by first sorting the array and then using a two-pointer strategy to find the pairs that sum to the negative of each element.

**Intuition:**
- Sort the array. This helps in easily skipping duplicates and using a two-pointer method effectively.
- Fix one number a[i] and then find two numbers from the remaining part of the array which sum to -a[i].
- Use two pointers starting from both ends of the remaining array to find the combinations.

```csharp
public class Solution {
    public IList<IList<int>> ThreeSum(int[] nums) {
        Array.Sort(nums); // Sort the array
        IList<IList<int>> result = new List<IList<int>>();

        for (int i = 0; i < nums.Length - 2; i++) {
            // Skip duplicates
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int left = i + 1, right = nums.Length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    result.Add(new List<int> { nums[i], nums[left], nums[right] });

                    // Skip duplicates for left and right
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;

                    left++;
                    right--;
                } else if (sum < 0) {
                    left++; // We need a larger sum, move the left pointer to the right
                } else {
                    right--; // We need a smaller sum, move the right pointer to the left
                }
            }
        }

        return result;
    }
}
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n), storing the result.

