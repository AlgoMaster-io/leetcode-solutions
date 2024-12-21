# [Leetcode 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approaches

1. [Basic Approach: Naive Increment](#basic-approach-naive-increment)
2. [Optimal Approach: Mathematical Insight](#optimal-approach-mathematical-insight)

## Basic Approach: Naive Increment

### Intuition
The basic idea is to repeatedly concatenate string A and check if B is now a substring. We start with an empty string and keep appending A to it until its length is at least as long as B. Then, we append A a few more times to account for cases where B could start at some point in A and circle around.

### Steps
1. Initialize an empty string `repeated_A`.
2. Start a counter `repeats` to keep track of how many times A has been appended.
3. Repeatedly append A to `repeated_A` and increment `repeats`.
4. Check if B is a substring of `repeated_A` after each appending.
5. If found, return the `repeats` count.
6. If not found by the time `repeated_A` is two times longer than B, return `-1` as it's clear B can't be formed.

```python
def repeatedStringMatch(A: str, B: str) -> int:
    repeated_A = ""
    repeats = 0
    # Repeat until the length is at least as long as B
    while len(repeated_A) < len(B) + len(A):
        repeated_A += A
        repeats += 1
        # Check if B is now a substring
        if B in repeated_A:
            return repeats
    
    # If all extensions fail, the task is impossible
    return -1
```

### Time Complexity
- The time complexity is O(n * m) where `n` is the length of string A and `m` is the length of string B, as each check for substring operation is O(m).

### Space Complexity
- The space complexity is O(n * k) where `k` is the maximum number of `A` repetitions needed to try and match `B`.

## Optimal Approach: Mathematical Insight

### Intuition
For B to be a substring of some repeated version of A, we need to consider:
- How many times A needs to repeat such that it's at least as long as B.
- Once A's length covers B, it might need one more repetition to cover cases where B spans across the end of one A and the start of another.

### Steps
1. Calculate the minimum number of times A needs to be repeated to be at least as long as B.
2. Construct this repetition and check if B is a substring.
3. If not, repeat A one more time for the above-explained border case and check again.
4. If still not found after these checks, return `-1`.

```python
def repeatedStringMatch(A: str, B: str) -> int:
    # Calculate the minimum repetitions needed
    min_repeats = (len(B) + len(A) - 1) // len(A)
    
    # Try that many repetitions
    repeated_A = A * min_repeats
    if B in repeated_A:
        return min_repeats
    
    # Try one more repetition for the border case
    repeated_A += A
    if B in repeated_A:
        return min_repeats + 1
    
    # If still not found, return -1
    return -1
```

### Time Complexity
- The time complexity is O(n + m) due to constructing the final repeated string and checking for the substring.

### Space Complexity
- The space complexity is O(n * ((m+n)/n)) = O(n + m), as we need space to store the repeated A that spans the possible occurrence of B.

Each approach incrementally improves upon understanding and optimizing the repetition of string A to efficiently check for the existence of string B within it.

