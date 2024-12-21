## Link to the Problem
[659. Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches
1. [Greedy with Hash Maps](#approach-1-greedy-with-hash-maps)

---

### Approach 1: Greedy with Hash Maps

#### Intuition
The problem requires us to determine if an array can be split into subsequences of at least 3 consecutive integers. A greedy strategy is essential here as for each element, we need to decide where to place it optimally to ensure future elements can also form valid subsequences.

To handle this:
- We'll use two hash maps:
  - `freq`: To record the frequency of each number in the array.
  - `need`: To determine how many numbers are needed to complete a subsequence.

For each number in the array:
- First, check if it can be appended to an existing subsequence (using the `need` map).
- If it cannot be appended to any existing subsequence, try to start a new subsequence of at least length 3.
- If neither of these strategies is possible, return `false`.

**Steps:**
1. Traverse through the sorted array.
2. For each number:
   - Use it if it completes an existing subsequence (check `need`).
   - If not, try to create a new subsequence starting with this number.
   - Adjust the `freq` and `need` based on these decisions.
3. If all numbers can form valid subsequences, return `true`.

#### C++ Code
```cpp
#include <unordered_map>
#include <vector>

bool isPossible(std::vector<int>& nums) {
    std::unordered_map<int, int> freq, need;

    // Build frequency map
    for (int num : nums) {
        freq[num]++;
    }

    // Iterate through nums
    for (int num : nums) {
        // If the current number is not available in the frequency map
        if (freq[num] == 0) continue;

        // If the current number is needed by some subsequence
        if (need[num] > 0) {
            // Decrease the need for this number and schedule need for the next number
            need[num]--;
            need[num + 1]++;
        }
        // Else, try to create a new subsequence starting with this number
        else if (freq[num + 1] > 0 && freq[num + 2] > 0) {
            freq[num + 1]--;
            freq[num + 2]--;
            need[num + 3]++;  // Need the next number to continue this subsequence
        }
        // If neither of the above options are possible, return false
        else {
            return false;
        }

        // Used one occurrence of the current number
        freq[num]--;
    }

    // If we complete the loop, it means we successfully split the array
    return true;
}
```

#### Time Complexity
- **O(n)**: We iterate through the nums array once. All operations related to hash maps take O(1) time on average.

#### Space Complexity
- **O(n)**: Additional space used for the freq and need hash maps.

This greedy method successfully ensures that each number is optimally placed to form valid subsequences of at least length 3, using efficient auxiliary data structures to track state without revisiting elements excessively.

