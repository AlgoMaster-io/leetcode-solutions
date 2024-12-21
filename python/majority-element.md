[169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approaches:

1. [Brute Force](#brute-force)
2. [Hash Map Counting](#hash-map-counting)
3. [Sorting](#sorting)
4. [Boyer-Moore Voting Algorithm](#boyer-moore-voting-algorithm)

### Brute Force

The idea here is to count the occurrences of each element, and if the count of a particular element is greater than `n/2`, we return that element as the majority element.

#### Intuition
Given that a majority element appears more than `n/2` times, checking the count of each element and comparing it to `n/2` will confirm the majority element.

```python
def majorityElement(nums):
    majority_count = len(nums) // 2
    for num in nums:
        count = sum(1 for elem in nums if elem == num)
        if count > majority_count:
            return num
```

**Time Complexity**: O(n^2)
- We iterate over the array and count the occurrences of each element, resulting in a nested loop.

**Space Complexity**: O(1)
- No additional space is used apart from variables.

---

### Hash Map Counting

Using a dictionary to count occurrences of each element in the list and verifying which element is the majority by comparing it to `n/2`.

#### Intuition
This approach leverages the use of a hash map to store counts, allowing for constant time complexity checks for the count of each element.

```python
def majorityElement(nums):
    counts = {}
    majority_count = len(nums) // 2
    for num in nums:
        if num in counts:
            counts[num] += 1
        else:
            counts[num] = 1
        if counts[num] > majority_count:
            return num
```

**Time Complexity**: O(n)
- Involves a single pass over the array with constant time operations for updating hash map.

**Space Complexity**: O(n)
- Dictionary to store counts of elements can potentially grow to the size of the input list.

---

### Sorting

If the array is sorted, the majority element must be present at the `n/2` index.

#### Intuition
Sorting the array positions the majority element at the middle index because it appears more than half of the time in the list.

```python
def majorityElement(nums):
    nums.sort()
    return nums[len(nums) // 2]
```

**Time Complexity**: O(n log n)
- Dominated by the sorting operation.

**Space Complexity**: O(1) or O(n) depending on the sorting algorithm used.
- Sorting in-place is O(1), but Python's built-in sort may require additional space.

---

### Boyer-Moore Voting Algorithm

This algorithm finds the majority element by maintaining a count, incrementing for the same element, and decrementing for different elements.

#### Intuition
The algorithm uses the fact that a majority element appears more than half the time in the list, allowing us to "vote" and cancel out non-majority elements.

```python
def majorityElement(nums):
    count = 0
    candidate = None

    for num in nums:
        if count == 0:
            candidate = num
        count += (1 if num == candidate else -1)

    return candidate
```

**Time Complexity**: O(n)
- A single pass over the list to find the candidate majority.

**Space Complexity**: O(1)
- Only a few variables are maintained regardless of input size.

---

These are the major approaches to solve the Majority Element problem. The brute force and hash map methods provide straightforward solutions, whereas sorting and the Boyer-Moore algorithm offer more optimal approaches.

