# Leetcode Problem: [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Binary Indexed Tree (Fenwick Tree)](#binary-indexed-tree-fenwick-tree)
- [Merge Sort Approach](#merge-sort-approach)

### Brute Force Approach

The brute force solution involves using a nested loop to count the number of smaller elements to the right of each element.

#### Intuition

- For each element, we iterate through all subsequent elements.
- We compare it with each element on its right and count how many are smaller.
- Store the count in the result list for that element.

#### Code
```python
def countSmaller(nums):
    n = len(nums)
    result = [0] * n
    for i in range(n):
        count = 0
        for j in range(i + 1, n):
            if nums[j] < nums[i]:
                count += 1
        result[i] = count
    return result
```

#### Complexity Analysis
- **Time Complexity**: \(O(n^2)\) where \(n\) is the number of elements in the input list. For each element, you check all subsequent elements.
- **Space Complexity**: \(O(n)\) for storing the result.

### Binary Indexed Tree (Fenwick Tree)

This approach utilizes a Fenwick Tree to maintain a cumulative frequency table for the elements and efficiently count the number of smaller elements to the right.

#### Intuition

- Discretize the element values to map them into indices of the Fenwick Tree.
- Traverse the list from the back, updating the tree and querying it to get the results.
- Update the position of the current element in the Fenwick Tree to record that it has occurred once.
- Find how many elements have been recorded in the tree that are smaller than the current element.

#### Code
```python
def countSmaller(nums):
    # Discretize nums to map into indices of Fenwick Tree
    sorted_nums = sorted(set(nums))
    ranks = {num: i + 1 for i, num in enumerate(sorted_nums)}

    def update(index, value, tree, size):
        while index <= size:
            tree[index] += value
            index += index & -index

    def query(index, tree):
        total = 0
        while index > 0:
            total += tree[index]
            index -= index & -index
        return total

    n = len(nums)
    result = []
    tree = [0] * (len(sorted_nums) + 1)
    
    # Traverse from the back
    for num in reversed(nums):
        rank = ranks[num]
        # Get the count of smaller numbers seen so far
        result.append(query(rank - 1, tree))
        # Add the current number to the tree
        update(rank, 1, tree, len(sorted_nums))
    
    result.reverse()  # Result is collected in reverse order
    return result
```

#### Complexity Analysis
- **Time Complexity**: \(O(n \log n)\). The Fenwick Tree operations (update and query) are logarithmic, and we perform them for each element.
- **Space Complexity**: \(O(n)\) for the Fenwick Tree and other auxiliary data structures.

### Merge Sort Approach

In this approach, we use a modified merge sort to count the number of smaller elements after each element.

#### Intuition

- As we merge two sorted halves, whenever an element from the right half is placed before an element from the left half, it implies this right half element is "smaller after" for all remaining left half elements.
- Modify the merge sort algorithm to accumulate such counts.

#### Code
```python
def countSmaller(nums):
    def merge_sort_enum(arr):
        half = len(arr) // 2
        if half:
            left, right = merge_sort_enum(arr[:half]), merge_sort_enum(arr[half:])
            for i in range(len(arr) - 1, -1, -1):
                if not right or left and left[-1][1] > right[-1][1]:
                    smaller[left[-1][0]] += len(right)
                    arr[i] = left.pop()
                else:
                    arr[i] = right.pop()
        return arr

    smaller = [0] * len(nums)
    merge_sort_enum(list(enumerate(nums)))
    return smaller
```

#### Complexity Analysis
- **Time Complexity**: \(O(n \log n)\). The merge sort process has a logarithmic recursive depth and \(O(n)\) merging work at each depth.
- **Space Complexity**: \(O(n)\) for storing indices and counts.

These solutions progressively improve both efficiency and complexity by leveraging concepts from computational geometry and algorithm optimization techniques.

