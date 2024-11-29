# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def findNumberOfLIS(self, nums):
        if not nums:
            return 0

        n = len(nums)
        lengths = [1] * n  # lengths[i] will be the length of the longest ending in nums[i]
        counts = [1] * n   # counts[i] will be the number of the longest ending in nums[i]

        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:
                    if lengths[j] + 1 > lengths[i]:
                        lengths[i] = lengths[j] + 1
                        counts[i] = counts[j]
                    elif lengths[j] + 1 == lengths[i]:
                        counts[i] += counts[j]

        max_length = max(lengths)
        numberOfLIS = sum(counts[i] for i in range(n) if lengths[i] == max_length)

        return numberOfLIS
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
class FenwickTree:
    def __init__(self, size):
        self.lengths = [0] * size
        self.counts = [0] * size

    def query(self, index):
        max_length, total_counts = 0, 0
        while index > 0:
            if self.lengths[index] > max_length:
                max_length = self.lengths[index]
                total_counts = self.counts[index]
            elif self.lengths[index] == max_length:
                total_counts += self.counts[index]
            index -= index & -index
        return max_length, total_counts

    def update(self, index, length, count):
        while index < len(self.lengths):
            if self.lengths[index] < length:
                self.lengths[index] = length
                self.counts[index] = count
            elif self.lengths[index] == length:
                self.counts[index] += count
            index += index & -index

class Solution:
    def findNumberOfLIS(self, nums):
        if not nums:
            return 0

        offset = 1
        sorted_unique_nums = sorted(set(nums))
        num_to_index = {num: idx + 1 for idx, num in enumerate(sorted_unique_nums)}

        fenwick_tree = FenwickTree(len(sorted_unique_nums) + offset)
        max_length, result = 0, 0

        for num in nums:
            current_index = num_to_index[num]
            previous = fenwick_tree.query(current_index - 1)
            current_length = previous[0] + 1
            current_count = max(previous[1], 1)

            fenwick_tree.update(current_index, current_length, current_count)

            if current_length > max_length:
                max_length = current_length
                result = current_count
            elif current_length == max_length:
                result += current_count

        return result
```

