# [Leetcode 15: 3Sum](https://leetcode.com/problems/3sum/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting and Two-pointer](#approach-2-sorting-and-two-pointer)

---

## Approach 1: Brute Force

### Intuition
The naive solution for the 3Sum problem is to try every possible combination of three numbers in the list and check if they sum up to zero. Although straightforward, this approach is computationally expensive as it involves checking every triplet in the array.

### Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        set<vector<int>> result; // Use set to avoid duplicate triplets
        
        // Iterate over each triplet
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        vector<int> triplet = {nums[i], nums[j], nums[k]};
                        sort(triplet.begin(), triplet.end()); // Sort to avoid duplicates
                        result.insert(triplet);
                    }
                }
            }
        }
        
        return vector<vector<int>>(result.begin(), result.end());
    }
};
```

### Time Complexity
- **O(N^3)**: Where N is the number of elements. There are three nested loops.
  
### Space Complexity
- **O(K)**: Where K is the number of unique triplets in the set.

---

## Approach 2: Sorting and Two-pointer

### Intuition
To efficiently solve the 3Sum problem, we can use the two-pointer technique on a sorted array. By fixing one element and finding the two other elements using two pointers, we can reduce unnecessary computations and avoid duplicates easily. This approach leverages sorting and the two-pointer technique to significantly reduce time complexity.

### Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        // Sort the vector to facilitate the two-pointer approach
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<vector<int>> result;
        
        for (int i = 0; i < n - 2; ++i) {
            if (i > 0 && nums[i] == nums[i - 1]) continue; // Skip duplicate elements
            
            int left = i + 1;
            int right = n - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                
                if (sum < 0) {
                    left++; // Move the left pointer to increase the sum
                } else if (sum > 0) {
                    right--; // Move the right pointer to decrease the sum
                } else {
                    result.push_back({nums[i], nums[left], nums[right]});
                    // Avoid duplicates for the second element
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    // Avoid duplicates for the third element
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                }
            }
        }
        
        return result;
    }
};
```

### Time Complexity
- **O(N^2)**: Where N is the number of elements. Sorting takes O(N log N) and the two-pointer scan takes O(N^2).

### Space Complexity
- **O(1)**: Additional space required is constant. The output space is not considered in the complexity analysis.

By using the sorting and two-pointer technique, we achieve a significant improvement in efficiency compared to the brute force approach, especially for larger datasets.

