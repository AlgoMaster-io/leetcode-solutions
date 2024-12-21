# [Leetcode 659: Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches
1. [Straightforward Counting Method](#straightforward-counting-method)
2. [Optimized Greedy Approach](#optimized-greedy-approach)

---

## Straightforward Counting Method
### Intuition:
The core idea is to count how many times each number appears in the array and try to form subsequences starting with the smallest number that appears at least once. This method leverages a dictionary to keep track of how many times each number is used in creating subsequences.

### Approach:
1. Count the frequency of each number in the nums array using a dictionary `freq`.
2. Initialize an empty dictionary `subs` to track the end of subsequences.
3. Traverse through each number:
   - If the number can be used to extend a subsequence, do so (i.e., if `subs[num-1] > 0`), decrease the count of `subs[num-1]` and increase `subs[num]`.
   - If the number can start a new subsequence (i.e., `freq[num+1] > 0` and `freq[num+2] > 0`), create a new subsequence.
4. If neither is possible, return False.
5. If we traverse the entire array successfully, return True.

### Code:
```python
def isPossible(nums):
    from collections import defaultdict

    freq = defaultdict(int)
    subs = defaultdict(int)

    # Count the frequency of each number
    for num in nums:
        freq[num] += 1

    for num in nums:
        if freq[num] == 0:
            continue

        # Use num to extend an existing subsequence
        if subs[num - 1] > 0:
            subs[num - 1] -= 1
            subs[num] += 1

        # Form a new subsequence num, num+1, num+2
        elif freq[num + 1] > 0 and freq[num + 2] > 0:
            freq[num + 1] -= 1
            freq[num + 2] -= 1
            subs[num + 2] += 1

        else:
            return False

        freq[num] -= 1

    return True
```

### Complexity:
- **Time Complexity**: \(O(n)\), where \(n\) is the length of the input array. We traverse the array a constant number of times.
- **Space Complexity**: \(O(n)\), used by the dictionaries `freq` and `subs`.

---

## Optimized Greedy Approach
### Intuition:
This approach is similar to the straightforward method but leverages the property of greedily forming as many valid consecutive sequences as possible. This is done by maintaining the state of possible subsequences more efficiently.

### Approach:
1. Use two dictionaries:
   - `freq`: to count the occurrences of each element.
   - `appendfreq`: to store count of subsequences that require a number to continue.
2. Iterate through each number:
   - If `freq[num] == 0`, continue to next number.
   - If a subsequence ending in `num-1` exists (i.e., `appendfreq[num-1] > 0`):
     - Decrease the requirement for `num-1` and increase it for `num`.
   - If no such subsequence exists, check if a new subsequence with `num`, `num+1`, and `num+2` can start.
   - If neither is possible, return False.
3. All checks pass, return True.

### Code:
```python
def isPossible(nums):
    from collections import defaultdict

    freq = defaultdict(int)
    appendfreq = defaultdict(int)

    for num in nums:
        freq[num] += 1

    for num in nums:
        if freq[num] == 0:
            continue
        elif appendfreq[num - 1] > 0:
            appendfreq[num - 1] -= 1
            appendfreq[num] += 1
        elif freq[num + 1] > 0 and freq[num + 2] > 0:
            freq[num + 1] -= 1
            freq[num + 2] -= 1
            appendfreq[num + 2] += 1
        else:
            return False
        freq[num] -= 1

    return True
```

### Complexity:
- **Time Complexity**: \(O(n)\), as we iterate over the list and perform constant time operations.
- **Space Complexity**: \(O(n)\), for maintaining two dictionaries similar in size to the input array length.

