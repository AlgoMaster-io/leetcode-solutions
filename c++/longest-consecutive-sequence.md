# [Leetcode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches
- [Approach 1: Brute Force Solution](#approach-1-brute-force-solution)
- [Approach 2: Sorting Solution](#approach-2-sorting-solution)
- [Approach 3: HashSet Optimized Solution](#approach-3-hashset-optimized-solution)

---

## Approach 1: Brute Force Solution

### Intuition:
The brute force approach involves understanding the problem in the simplest terms: for each element, determine the maximum sequence starting with that element. This requires checking in a linear fashion if each number is part of a consecutive sequence.

### Solution
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int longestStreak = 0;
        for (int num : nums) {
            // Start counting length for the current number
            int currentNum = num;
            int currentStreak = 1;

            // Try to find consecutive numbers
            while (find(nums.begin(), nums.end(), currentNum + 1) != nums.end()) {
                currentNum += 1;
                currentStreak += 1;
            }

            // Update the maximum streak length
            longestStreak = max(longestStreak, currentStreak);
        }
        return longestStreak;
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(n^2)\), where \(n\) is the number of elements in the array. The `find` operation takes \(O(n)\) time for each element.
- **Space Complexity**: \(O(1)\), no additional data structures are used beyond the input itself.

---

## Approach 2: Sorting Solution

### Intuition:
Sorting the array allows us to easily determine sequences by simply iterating through the sorted elements and checking for consecutive differences.

### Solution
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;

        sort(nums.begin(), nums.end());

        int longestStreak = 1;
        int currentStreak = 1;

        for (int i = 1; i < nums.size(); i++) {
            // Skip duplicates to maintain the streak count correctly
            if (nums[i] != nums[i - 1]) {
                if (nums[i] == nums[i - 1] + 1) {
                    currentStreak += 1;
                } else {
                    longestStreak = max(longestStreak, currentStreak);
                    currentStreak = 1;
                }
            }
        }
        return max(longestStreak, currentStreak); // in case the longest is at the end
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(n \log n)\), due to the sort operation on the array.
- **Space Complexity**: \(O(1)\), since the algorithm only uses a constant amount of additional space.

---

## Approach 3: HashSet Optimized Solution

### Intuition:
Using a HashSet allows you to efficiently check for the presence of consecutive numbers without needing to sort the array. The idea is to add all numbers to a set and only start counting a sequence from numbers that aren't a continuation (i.e., numbers that aren't preceded by any in the set).

### Solution
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> numSet(nums.begin(), nums.end());

        int longestStreak = 0;

        for (int num : numSet) {
            // Ignore numbers that are not the start of a sequence
            if (!numSet.count(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // Check how long the streak can go on
                while (numSet.count(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(n)\), since each number is processed at most twice (once when adding to the set, once when iterating).
- **Space Complexity**: \(O(n)\), to store the set of numbers.

This final approach is the most optimal due to its linear time complexity and ability to handle large inputs efficiently.

