# [659. Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches:
- [Greedy with Hash Maps](#greedy-with-hash-maps)

### Greedy with Hash Maps

#### Intuition:
To solve the problem of splitting the array `nums` into consecutive subsequences of length at least 3, we can leverage a greedy algorithm. The idea is to always extend existing subsequences if possible before starting a new subsequence. This ensures that shorter subsequences do not remain, which would violate the problem conditions.

We use two hash maps:
1. **Count map** (`count`): Keeps track of the frequency of each number in the input array.
2. **Tail map** (`tail`): Stores the count of subsequences ending with a particular number. 

The steps are:
1. Traverse the sorted `nums` array.
2. For each number (`num`), check its frequency in the `count` map.
3. If it is part of a subsequence ending with `num - 1`, extend the existing subsequence by decrementing the count in the `tail` map for `num - 1` and incrementing for `num`.
4. If not, try to start a new subsequence with `num`, `num + 1`, `num + 2` by checking their frequencies.
5. If neither extending nor creating is possible, return `false`.
6. Decrement the count for the used number in `count` map.

This clever usage of greedy strategy ensures that the formation of subsequences is optimal at each step.

#### Code:
```javascript
function isPossible(nums) {
    const count = new Map();
    const tail = new Map();
    
    // Fill the frequency map
    for (const num of nums) {
        count.set(num, (count.get(num) || 0) + 1);
    }
    
    for (const num of nums) {
        // If there are no more 'num' left, skip.
        if (count.get(num) === 0) continue;

        // If a subsequence ending with 'num - 1' can be extended
        if ((tail.get(num - 1) || 0) > 0) {
            tail.set(num - 1, tail.get(num - 1) - 1);
            tail.set(num, (tail.get(num) || 0) + 1);
        } else if ((count.get(num + 1) || 0) > 0 && (count.get(num + 2) || 0) > 0) {
            // If we can start a new subsequence with num, num + 1, num + 2
            count.set(num + 1, count.get(num + 1) - 1);
            count.set(num + 2, count.get(num + 2) - 1);
            tail.set(num + 2, (tail.get(num + 2) || 0) + 1);
        } else {
            // Neither can extend nor start a new subsequence
            return false;
        }
        
        // Decrement the current number's frequency, as we used it
        count.set(num, count.get(num) - 1);
    }
    
    return true;
}
```

- **Time Complexity:** O(n), where n is the length of the `nums` array, since we traverse the list and perform constant-time operations with the maps.
- **Space Complexity:** O(n), due to the hash maps used to store the frequency and tail information for the elements in the array.

