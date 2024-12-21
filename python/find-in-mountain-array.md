# [Leetcode 1095: Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Approaches
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Binary Search with Peak Finding](#approach-2-binary-search-with-peak-finding)

---

## Approach 1: Linear Search

### Intuition
In this approach, we perform a straightforward linear search to find the target. Since the array is first strictly increasing and then strictly decreasing, we traverse up to the peak and check for the target. If the target is not found, we continue the search in a decreasing manner until we find it or reach the end of the array.

### Steps
1. Traverse the mountain array from left to right up to the peak while checking for the target.
2. If the target is found in the increasing part, return the index.
3. If the target is not found, continue to search in the decreasing part of the array beginning from the peak.
4. If the target is found in the decreasing part, return the index. Otherwise, return -1.

### Time Complexity
- **O(n)**: We potentially need to traverse the entire array to find the target if it exists.

### Space Complexity
- **O(1)**: We use a constant amount of extra space.

```python
class Solution:
    def findInMountainArray(self, target: int, mountain_arr: 'MountainArray') -> int:
        # Get the length of the MountainArray
        length = mountain_arr.length()
        
        # Iterate over the array up to the peak
        for i in range(length):
            # Get the current element
            value = mountain_arr.get(i)
            # Check if current element is the target
            if value == target:
                return i
            # If we detect the peak, we break and start from the other side
            if i > 0 and value < mountain_arr.get(i - 1):
                break
                
        # Continue searching starting from the peak downwards
        for i in range(length - 1, -1, -1):
            # Get the current element
            value = mountain_arr.get(i)
            # Check if current element is the target
            if value == target:
                return i
        
        # If target is not found, return -1
        return -1
```

---

## Approach 2: Binary Search with Peak Finding

### Intuition
Utilize the properties of the mountain array to efficiently find the target using binary search. First, identify the peak of the mountain, then perform binary search twice: once in the increasing subarray and once in the decreasing subarray.

### Steps
1. Identify the peak index using a binary search.
2. Perform a binary search from the start of the array up to the peak to find the target in the increasing part.
3. If the target is not found, perform a binary search from the peak to the end of the array to find the target in the decreasing part.

### Time Complexity
- **O(log n)**: We perform two binary searches, each consuming logarithmic time in terms of the array length.

### Space Complexity
- **O(1)**: We use a constant amount of extra space.

```python
class Solution:
    def findInMountainArray(self, target: int, mountain_arr: 'MountainArray') -> int:
        def binary_search(left, right, asc):
            while left <= right:
                mid = left + (right - left) // 2
                mid_val = mountain_arr.get(mid)
                if mid_val == target:
                    return mid
                if asc:
                    if mid_val < target:
                        left = mid + 1
                    else:
                        right = mid - 1
                else:
                    if mid_val < target:
                        right = mid - 1
                    else:
                        left = mid + 1
            return -1
        
        # Finding the peak index
        left, right = 0, mountain_arr.length() - 1
        while left < right:
            mid = left + (right - left) // 2
            if mountain_arr.get(mid) < mountain_arr.get(mid + 1):
                left = mid + 1
            else:
                right = mid
        peak = left
        
        # Binary search on the increasing part
        index = binary_search(0, peak, asc=True)
        if index != -1:
            return index
        
        # Binary search on the decreasing part
        return binary_search(peak + 1, mountain_arr.length() - 1, asc=False)
```

By employing binary search, we significantly improve the efficiency of finding our target and leverage the mountain's structure effectively.

