# [Leetcode 169: Majority Element](https://leetcode.com/problems/majority-element/)

## Approaches
1. [Brute Force Approach](#approach1)
2. [Sorting Approach](#approach2)
3. [Hash Map Approach](#approach3)
4. [Boyer-Moore Voting Algorithm](#approach4)

---

## Approach 1: Brute Force Approach

The brute force approach involves iterating through each element in the array and counting its occurrences. If an element occurs more than `n/2` times, it is the majority element. 

#### Intuition
We capture every element's frequency and determine if its frequency is greater than half the size of the array. Although inefficient, it demonstrates the basic concept of checking every possibility.

### C++ Code
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            int count = 0;
            // Count occurrences of nums[i]
            for (int j = 0; j < n; ++j) {
                if (nums[j] == nums[i]) {
                    count++;
                }
            }
            // If count is more than n/2, we've found our majority element
            if (count > n / 2) {
                return nums[i];
            }
        }
        return -1; // Majority element must exist, this is a safeguard
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), where n is the number of elements in the array.
- **Space Complexity**: O(1), no extra space used.

---

## Approach 2: Sorting Approach

By sorting the array, the majority element must be at the position `n/2`, provided a zero-based index.

#### Intuition
When the array is sorted, the majority element will always be in the center portion of the array due to its frequency being greater than `n/2`.

### C++ Code
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n log n), due to the sorting operation.
- **Space Complexity**: O(1), if we disregard the space used by the internal sorting algorithm.

---

## Approach 3: Hash Map Approach

A hash map can store counts of each element as we iterate over the array and quickly identify when a majority is reached.

#### Intuition
A hash map allows us to efficiently count the occurrences of each number while traversing the array only once. 

### C++ Code
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> countMap;
        int n = nums.size();
        for (int num : nums) {
            countMap[num]++;
            // If count of any number exceeds n/2, return the number
            if (countMap[num] > n / 2) {
                return num;
            }
        }
        return -1; // Majority element must exist, this is a safeguard
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of elements in the array.
- **Space Complexity**: O(n), for storing frequencies in the hash map.

---

## Approach 4: Boyer-Moore Voting Algorithm

The Boyer-Moore Voting Algorithm is a two-step process designed to find the majority element in linear time and constant space.

#### Intuition
The algorithm involves maintaining a candidate count for a potential majority element, incrementing and decrementing its count based on whether we encounter the candidate or not. By the time we traverse the list, the candidate will hold the majority since its count will end positive due to its required majority characteristic.

### C++ Code
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 0;
        int candidate = 0;

        for (int num : nums) {
            // Reset the candidate when count is 0
            if (count == 0) {
                candidate = num;
            }
            // Increment or decrement the count based on the candidate match
            count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), going through the array once to determine the majority element.
- **Space Complexity**: O(1), constant space used. 

This last approach is optimal for this problem, effectively finding the majority element without any extra space (beyond variables) and in linear time.

