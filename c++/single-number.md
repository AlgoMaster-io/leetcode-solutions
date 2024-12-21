# [Leetcode 136: Single Number](https://leetcode.com/problems/single-number/)

## Approaches
- [Approach 1: Brute Force Using Nested Loops](#approach-1)
- [Approach 2: Using Sorting](#approach-2)
- [Approach 3: Using Hash Table](#approach-3)
- [Approach 4: Bit Manipulation (Optimal Solution)](#approach-4)

---

## Approach 1: Brute Force Using Nested Loops

### Intuition
The basic idea is to compare each element with every other element in the array. For the element which is not repeated in the list, will not find its pair.

### Algorithm
1. Loop through each element in the array.
2. For each element, check if it has an identical counterpart elsewhere in the array.
3. If not, return that element.

### Code
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        for(int i = 0; i < nums.size(); i++) {
            bool found = false;
            for(int j = 0; j < nums.size(); j++) {
                if (i != j && nums[i] == nums[j]) {
                    found = true;
                    break;
                }
            }
            if (!found) {
                return nums[i];
            }
        }
        return -1; // This should not happen if input is guaranteed to have a single number.
    }
};
```

### Complexity
- **Time Complexity:** O(n^2), where n is the number of elements in the nums array. This is because for each element, we potentially look at every other element.
- **Space Complexity:** O(1), as we do not use any additional data structures.

---

## Approach 2: Using Sorting

### Intuition
By sorting the array, any duplicates will be adjacent to each other. Thus, by traversing the sorted array, any non-adjacent number is our single number.

### Algorithm
1. Sort the array.
2. Traverse the array and check adjacent elements. If they are not equal, return the current element.

### Code
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end()); // Sort the array
        for(int i = 0; i < nums.size() - 1; i += 2) {
            if(nums[i] != nums[i+1]) {
                return nums[i];
            }
        }
        // If we reached the end, the last element is the single number.
        return nums.back();
    }
};
```

### Complexity
- **Time Complexity:** O(n log n), due to the sorting step.
- **Space Complexity:** O(1), as we are sorting the array in-place.

---

## Approach 3: Using Hash Table

### Intuition
We can use a hash table to count occurrences of each element. The element with a count of 1 is our answer.

### Algorithm
1. Traverse the array and store count of each number in a hash table.
2. Traverse the hash table and return the element with a count of 1.

### Code
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int, int> countMap;
        // Count each number
        for(int num : nums) {
            countMap[num]++;
        }
        // Find the single number
        for(auto& pair : countMap) {
            if(pair.second == 1) {
                return pair.first;
            }
        }
        return -1; // This should not happen with a valid input
    }
};
```

### Complexity
- **Time Complexity:** O(n), we go through the array twice, once to count and once to find the single number.
- **Space Complexity:** O(n), to store the counts in the hash table.

---

## Approach 4: Bit Manipulation (Optimal Solution)

### Intuition
Using XOR properties can solve the problem efficiently:
- x ^ x = 0
- x ^ 0 = x
- XOR is commutative and associative.
Thus, XORing all given numbers will cancel out all duplicate numbers, leaving only the single number.

### Algorithm
1. Initialize a variable `result` to 0.
2. XOR every element in the array with `result`.
3. The result will be the single number.

### Code
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for(int num : nums) {
            result ^= num; // XOR current number with result
        }
        return result;
    }
};
```

### Complexity
- **Time Complexity:** O(n), as we process each element once.
- **Space Complexity:** O(1), no extra space needed beyond the `result` variable.

This approach is optimal and recommended for its simplicity and efficiency.

