# [LeetCode 659: Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches
- [Approach 1: Greedy with Frequency and Append Maps](#approach-1-greedy-with-frequency-and-append-maps)

### Approach 1: Greedy with Frequency and Append Maps

**Intuition:**

The task is to determine if we can split a given array into consecutive subsequences of length at least 3. The approach involves using two hash maps:
1. `frequency` map to keep track of the remaining occurrences of each element.
2. `appendNeeded` map to track the need to append an element to a previously formed subsequence.

The basic greedy strategy is:
- We iterate through the array and for each element `num`:
  - First, check if `num` can be added to an existing subsequence where it can help form a valid consecutive sequence.
  - If `num` can't be added to any existing subsequence, try to start a new subsequence from it.
  - If neither is possible, we should return false because it is impossible to form the desired subsequences.

**Steps in the code:**
1. Use a `frequency` map to track the available counts of each number.
2. Use an `appendNeeded` map to track if a number needs to be the next element in a subsequence.
3. Loop through each number `num` in the input:
   - If the `frequency` of `num` is zero, continue, meaning it's already used.
   - If `num` can be appended to a subsequence (i.e., `appendNeeded[num] > 0`), reduce the need and increase the need for the next number.
   - If `num` can't be appended, try starting a new subsequence `num, num+1, num+2` and update the necessary maps.
   - If neither appending to a subsequence nor starting one is possible, return false.
4. If we can iterate through without issue, return true.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean isPossible(int[] nums) {
        // Frequency map to track counts of each number
        Map<Integer, Integer> frequency = new HashMap<>();
        // To track where an element is needed to continue a sequence
        Map<Integer, Integer> appendNeeded = new HashMap<>();

        // Populate the frequency map
        for (int num : nums) {
            frequency.put(num, frequency.getOrDefault(num, 0) + 1);
        }

        // Iterate over the array
        for (int num : nums) {
            if (frequency.get(num) == 0) {
                continue;
            }

            // If there is a requirement to append num to a sequence
            if (appendNeeded.getOrDefault(num, 0) > 0) {
                // Reduce the need and increase the need for num+1
                appendNeeded.put(num, appendNeeded.get(num) - 1);
                appendNeeded.put(num + 1, appendNeeded.getOrDefault(num + 1, 0) + 1);
            }
            // Try to create a new sequence [num, num+1, num+2]
            else if (frequency.getOrDefault(num + 1, 0) > 0 && frequency.getOrDefault(num + 2, 0) > 0) {
                // Use num, num+1 and num+2
                frequency.put(num + 1, frequency.get(num + 1) - 1);
                frequency.put(num + 2, frequency.get(num + 2) - 1);
                // Now num+3 is awaited
                appendNeeded.put(num + 3, appendNeeded.getOrDefault(num + 3, 0) + 1);
            }
            // If none of the above actions are possible, return false
            else {
                return false;
            }

            // Decrease frequency of current number as it's used
            frequency.put(num, frequency.get(num) - 1);
        }

        // Returning true as all numbers can be split into subsequences
        return true;
    }
}
```

- **Time Complexity:** O(n), where n is the number of elements in the input array. Each element is processed a constant number of times.
- **Space Complexity:** O(n), as we use extra space for the frequency and appendNeeded maps.

