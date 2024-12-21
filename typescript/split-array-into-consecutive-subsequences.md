# [Leetcode 659: Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches

- [Greedy Approach with Hashmaps](#greedy-approach-with-hashmaps)

### Greedy Approach with Hashmaps

The goal is to figure out a way to determine whether the entire array can be split into multiple consecutive subsequences, each of which has a length of at least 3.

**Intuition**:  
To achieve this, we can employ a greedy algorithm using hashmaps. The idea is to try to extend existing subsequences by adding numbers to them or starting new ones when it's not possible to extend any.

**Steps**:
1. **Create Frequency Map**: 
   - We will create a frequency map (`freq`) to count the occurrences of each number in the array.

2. **Using a Need Map**:
   - Another hashmap (`need`) keeps track of the numbers we need to complete current subsequences.
   
3. **Iterate Over Array**:
   - For each number in the sorted array:
     - **Skip used numbers**: If a number is already used up (frequency is zero), skip it.
     - **Check `need` map**: If the number is needed to extend a subsequence, reduce the count in the `need` map and accordingly adjust for the next number in the sequence.
     - **Start new subsequence**: If it can't be part of an existing sequence, check if it can start a new subsequence of length at least 3.
     - **Update frequency map**: Adjust the frequency map as numbers are used.

**Time Complexity**: O(n) - where n is the number of elements in nums, because we iterate over the array and perform constant-time operations within the iteration.

**Space Complexity**: O(n) - due to the extra space used by the frequency and need maps.

```typescript
function isPossible(nums: number[]): boolean {
    // frequency map to count occurrences of each number
    const freq: Map<number, number> = new Map();
    // need map to track numbers required to extend existing subsequences
    const need: Map<number, number> = new Map();

    // initialize the frequency map
    for (const num of nums) {
        freq.set(num, (freq.get(num) || 0) + 1);
    }

    for (const num of nums) {
        if (freq.get(num) === 0) {
            // if frequency is zero, this number is exhausted
            continue;
        } else if ((need.get(num) || 0) > 0) {
            // if the number is needed to extend an existing subsequence
            need.set(num, need.get(num)! - 1);  // reduce need count for this number
            need.set(num + 1, (need.get(num + 1) || 0) + 1); // now, we need the next number
        } else if ((freq.get(num + 1) || 0) > 0 && (freq.get(num + 2) || 0) > 0) {
            // if the number can start a new subsequence with next two numbers available
            freq.set(num + 1, freq.get(num + 1)! - 1); // reduce count for num+1
            freq.set(num + 2, freq.get(num + 2)! - 1); // reduce count for num+2
            need.set(num + 3, (need.get(num + 3) || 0) + 1); // subsequently, we will need num+3
        } else {
            return false; // can't use this number to form or extend a subsequence
        }
        // reduce frequency of current number as it's used
        freq.set(num, freq.get(num)! - 1);
    }

    return true;
}
```

In this approach, we efficiently check and manage the formation and extension of subsequences while maintaining necessary conditions through the use of two hashmaps.

