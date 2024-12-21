## [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting](#approach-2-sorting)
- [Approach 3: Radix Sort + Linear Scan](#approach-3-radix-sort--linear-scan)
- [Approach 4: Bucket Sort (Pigeonhole Principle)](#approach-4-bucket-sort-pigeonhole-principle)

---

### Approach 1: Brute Force

The simplest and least efficient method is to calculate the difference between every pair of numbers in the sorted array and then select the maximum.

#### Intuition:
- Iterate through every possible pair of distinct elements in the array.
- Calculate the difference between each pair.
- Keep track of the maximum difference observed.

```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;

        int maxGap = 0;
        // Check every pair of numbers
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // Update maxGap if a larger gap is found
                maxGap = max(maxGap, abs(nums[i] - nums[j]));
            }
        }
        return maxGap;
    }
};
```

**Time Complexity:** O(n^2), where n is the number of elements in the array, as we check every pair.

**Space Complexity:** O(1), as no additional space is used apart from the maximum gap variable.

---

### Approach 2: Sorting

By sorting the array, the problem of finding the maximum gap can be reduced to checking consecutive elements.

#### Intuition:
- Sort the array first.
- After sorting, the maximum gap will be between two consecutive elements in the array.
- Thus, iterate through the sorted array and calculate the differences between consecutive numbers. Track the maximum.

```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;

        // First, sort the array
        sort(nums.begin(), nums.end());
        
        int maxGap = 0;
        // Compute gaps between consecutive elements
        for (int i = 1; i < n; i++) {
            maxGap = max(maxGap, nums[i] - nums[i - 1]);
        }
        return maxGap;
    }
};
```

**Time Complexity:** O(n log n) for sorting the array.

**Space Complexity:** O(1) or O(n) depending on the sorting algorithm used (if it is in-place or not).

---

### Approach 3: Radix Sort + Linear Scan

Use Radix Sort to achieve linearithmic time complexity, followed by a linear scan.

#### Intuition:
- First apply radix sort on the array.
- Then, like the previous sorting approach, find the maximum gap between consecutive elements in the sorted result.

```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;

        // Perform radix sort
        int maxNum = *max_element(nums.begin(), nums.end());
        int exp = 1;
        
        vector<int> buffer(n);
        while (maxNum / exp > 0) {
            vector<int> count(10, 0);
            
            // Counting occurrences
            for (int num : nums) {
                count[(num / exp) % 10]++;
            }
            
            // Building position map
            for (int i = 1; i < 10; i++) {
                count[i] += count[i - 1];
            }
            
            // Building sorted buffer
            for (int i = n - 1; i >= 0; i--) {
                buffer[--count[(nums[i] / exp) % 10]] = nums[i];
            }
            
            // Copy the sorted numbers back to the original array
            nums = buffer;
            exp *= 10;
        }
        
        int maxGap = 0;
        // Compute gaps between consecutive elements
        for (int i = 1; i < n; i++) {
            maxGap = max(maxGap, nums[i] - nums[i - 1]);
        }
        return maxGap;
    }
};
```

**Time Complexity:** O(n) for radix sort when considering the number of digits in each number is constant, followed by O(n) for the linear scan.

**Space Complexity:** O(n) for the auxiliary array used during radix sort.

---

### Approach 4: Bucket Sort (Pigeonhole Principle)

This approach utilizes the Pigeonhole Principle by distributing numbers into buckets, such that the maximum gap is only between numbers in successive buckets rather than within a bucket.

#### Intuition:
- Calculate the minimum and maximum elements in the array.
- Use these to determine a reasonable bucket size: `(max_value - min_value) / (n - 1)`.
- Place each number into a bucket based on its value.
- Only compare max of current bucket with min of next bucket, not within buckets.

```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;

        // Find the minimum and maximum numbers
        int minValue = *min_element(nums.begin(), nums.end());
        int maxValue = *max_element(nums.begin(), nums.end());

        // Calculate the gap, and initially it is minimum
        int bucketSize = max(1, (maxValue - minValue) / (n - 1));

        // Number of buckets
        int bucketCount = (maxValue - minValue) / bucketSize + 1;
        
        // Initialize the buckets with empty states
        vector<pair<int, int>> buckets(bucketCount, {-1, -1});
        
        // Filling the buckets
        for (int num : nums) {
            int bucketIdx = (num - minValue) / bucketSize;
            if (buckets[bucketIdx].first == -1) {
                buckets[bucketIdx].first = buckets[bucketIdx].second = num;
            } else {
                buckets[bucketIdx].first = min(buckets[bucketIdx].first, num);
                buckets[bucketIdx].second = max(buckets[bucketIdx].second, num);
            }
        }
        
        // Find maximum gap
        int maxGap = 0; 
        int prevMax = minValue; // Cycle through filled buckets to find maximum gap
        for (int i = 0; i < bucketCount; i++) {
            if (buckets[i].first == -1) continue;  // Skip empty buckets

            // Calculate gap between this and the previous bucket
            maxGap = max(maxGap, buckets[i].first - prevMax);
            prevMax = buckets[i].second;
        }
        return maxGap;
    }
};
```

**Time Complexity:** O(n), as the problem of distribution into buckets and subsequent single pass ensures linear time complexity.

**Space Complexity:** O(n), needed for the buckets to facilitate pigeonhole inspection.

---

In conclusion, while the brute force approach is intuitive, it lacks efficiency. More optimal solutions leverage sorting with direct gap inspection or advanced techniques such as radix and bucket sort to achieve linear or near-linear time complexities.

