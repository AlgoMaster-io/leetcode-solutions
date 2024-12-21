# [LeetCode 167: Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Two Pointers](#approach-2-two-pointers)

---

## Approach 1: Brute Force

### Intuition
The simplest approach to solve this problem is to iterate over each pair of numbers and check if they add up to the target. Given the array is sorted, once we find a pair with a sum equal to the target, we can immediately return their indices.

### Detailed Steps
1. Iterate through the array with a nested loop.
2. For each pair `(i, j)`, where `i < j`, check if `numbers[i] + numbers[j]` equals the target.
3. If it does, return `[i + 1, j + 1]` because the result should be 1-based.

This approach considers every possible pair and checks the sum, which is quite inefficient in terms of time complexity.

### Complexity Analysis
- **Time Complexity:** \(O(n^2)\), where \(n\) is the number of elements in the input array. This is because we are checking every possible pair.
- **Space Complexity:** \(O(1)\), since no additional space is used apart from variables for iteration.

### C++ Code

```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    for (int i = 0; i < numbers.size(); ++i) {
        for (int j = i + 1; j < numbers.size(); ++j) {
            // Check if the current pair sums to the target
            if (numbers[i] + numbers[j] == target) {
                // Return indices as 1-based index
                return {i + 1, j + 1};
            }
        }
    }
    // Return an empty vector if no solution is found
    return {};
}
```

---

## Approach 2: Two Pointers

### Intuition
Utilizing the sorted nature of the array, a more optimal approach can be utilized using two pointers. By employing a two-pointer technique, we can reduce the time complexity by efficiently narrowing down potential pairs without exhaustive enumeration.

### Detailed Steps
1. Initialize two pointers, one at the beginning (`left`) and one at the end (`right`) of the array.
2. Calculate the sum of the values at these two pointers.
3. If the sum equals the target, return the indices of the two pointers incremented by one.
4. If the sum is less than the target, move the `left` pointer to the right to increase the sum.
5. If the sum is greater than the target, move the `right` pointer to the left to decrease the sum.
6. Repeat until the pointers meet or the solution is found.

### Complexity Analysis
- **Time Complexity:** \(O(n)\), where \(n\) is the number of elements in the input array. Each element is processed at most once by each of the two pointers.
- **Space Complexity:** \(O(1)\), since we only use two extra variables for the pointers.

### C++ Code

```cpp
#include <vector>
using namespace std;

vector<int> twoSum(vector<int>& numbers, int target) {
    int left = 0, right = numbers.size() - 1;
    while (left < right) {
        // Calculate the sum of the two pointers
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            // Return indices as 1-based index
            return {left + 1, right + 1};
        } else if (sum < target) {
            // Move left pointer right to increase sum
            left++;
        } else {
            // Move right pointer left to decrease sum
            right--;
        }
    }
    // Return an empty vector if no solution is found
    return {};
}
```

In summary, the two-pointer approach effectively finds the required indices in linear time by leveraging the sorted order of the input array, making it a more efficient solution than the brute force method.

