# [Leetcode Problem 15: 3Sum](https://leetcode.com/problems/3sum/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Two Pointers Approach](#two-pointers-approach)

---

### Brute Force Approach
The simplest approach is to try every possible triplet in the array and check if their sum is zero. Although this guarantees we find all solutions, it is not efficient.

#### Intuition
1. We iterate through each element, and for each element, iterate over every pair of subsequent elements.
2. We check if the sum of these three elements is zero.
3. If the sum is zero, we add the triplet to our list of solutions.
4. To ensure we do not add duplicate triplets, we use a set to store the triplets.

#### Code
```java
import java.util.*;

public class ThreeSum {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> triplets = new HashSet<>();
        // Check all combinations of triplets
        for (int i = 0; i < nums.length - 2; i++) {
            for (int j = i + 1; j < nums.length - 1; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    // If their sum is zero, add sorted triplet to the set
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(triplet);
                        triplets.add(triplet);
                    }
                }
            }
        }
        return new ArrayList<>(triplets);
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^3) - We have three nested loops.
- **Space Complexity**: O(n) - In the worst case, the set can store all unique triplets.

---

### Two Pointers Approach
The two-pointers approach is more efficient. By sorting the array and using two pointers, we reduce the time complexity significantly.

#### Intuition
1. First, sort the array. This helps in avoiding duplicates and allows the use of two pointers.
2. Iterate through the sorted array with an index `i`.
3. For each `i`, initialize two pointers: `left` at `i+1` and `right` at the end of the array.
4. Calculate the sum of the elements at `i`, `left`, and `right`.
5. If the sum is zero, add the triplet to the results and move both pointers inward, skipping duplicates.
6. If the sum is less than zero, move the `left` pointer to increase the sum.
7. If the sum is greater than zero, move the `right` pointer to decrease the sum.

#### Code
```java
import java.util.*;

public class ThreeSum {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums); // Sort the array first
        
        for (int i = 0; i < nums.length - 2; i++) {
            // To avoid duplicates
            if (i == 0 || nums[i] != nums[i - 1]) {
                int left = i + 1, right = nums.length - 1;
                while (left < right) {
                    int sum = nums[i] + nums[left] + nums[right];
                    if (sum == 0) {
                        result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                        // Skip duplicates for `left` and `right`
                        while (left < right && nums[left] == nums[left + 1]) left++;
                        while (left < right && nums[right] == nums[right - 1]) right--;
                        left++;
                        right--;
                    } else if (sum < 0) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^2) - Sorting the array is O(n log n), and the two-pointers approach takes O(n^2).
- **Space Complexity**: O(1) - No additional space is used other than the output list.

