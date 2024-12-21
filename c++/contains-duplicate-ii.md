### [Leetcode Problem 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

#### Approaches:
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Hash Map Approach](#approach-2-hash-map-approach)

---

#### Approach 1: Brute Force Approach

##### Intuition:
The simplest approach is to check every possible pair of elements in the array and determine if they're equal and if their indices_difference is within the given `k`. Although not optimal, this will cover all cases.

##### Steps:
1. Loop through each element using two nested loops.
2. In the outer loop, iterate through each element as the first element of the pair.
3. For each element in the outer loop, check all possible pairs with elements ahead of it in the array with the inner loop.
4. Compare the values of the elements and their index difference.
5. If the values are equal and the difference in indices is less than or equal to `k`, return `true`.
6. If you check all pairs and find none matching the criteria, return `false`.

```cpp
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    int n = nums.size();
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            // Check if values are the same and index difference within k
            if (nums[i] == nums[j] && j - i <= k) {
                return true;
            }
        }
    }
    return false;
}
```

##### Time Complexity:
- O(n^2): Due to the nested loops, the algorithm checks every possible pair of elements in the array.

##### Space Complexity:
- O(1): No additional space beyond input variables is used.

---

#### Approach 2: Hash Map Approach

##### Intuition:
By using a hash map (unordered_map), we can track the last index where each value was observed. For each new element, we can quickly check if it is a duplicate and whether its duplicate index is within a distance `k`. This significantly reduces the time complexity by avoiding unnecessary comparisons.

##### Steps:
1. Initialize an empty hash map to store the value and its last seen index.
2. Loop through the list of numbers.
3. For each number, check if it is already present in the hash map.
4. If present, compare the current index with the stored index in the hash map:
   - If the difference (current index - stored index) is less than or equal to `k`, return `true`.
   - Otherwise, update the stored index to the current index.
5. If not present, add the number with its current index to the hash map.
6. If no duplicates are found within the required distance, return `false`.

```cpp
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_map<int, int> numIndexMap;
    for (int i = 0; i < nums.size(); ++i) {
        // Check if the number is seen and the index difference is within k
        if (numIndexMap.find(nums[i]) != numIndexMap.end() && i - numIndexMap[nums[i]] <= k) {
            return true;
        }
        // Update or add the number's latest index
        numIndexMap[nums[i]] = i;
    }
    return false;
}
```

##### Time Complexity:
- O(n): Each element is processed once, and each map operation (insertion, lookup) is O(1) on average.

##### Space Complexity:
- O(min(n, k)): The space for the hash map is limited to the number of elements up to `k`, which is the maximum number of distinct elements it would store concurrently. Hence, in the worst case, it's O(n) but limited by `k`.

---

By using these methods, we can effectively solve the problem of finding duplicates within a specified range in an array, moving from a simple brute force method to an optimized hash map solution.

