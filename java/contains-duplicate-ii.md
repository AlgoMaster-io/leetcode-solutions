# [Leetcode 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window with HashSet](#sliding-window-with-hashset)
3. [Sliding Window with HashMap](#sliding-window-with-hashmap)
---

## Brute Force Approach

### Intuition
The simplest approach to solve this problem is to check every pair of elements in the array. If two elements are the same and are within the given range `k` from each other, we return true. Otherwise, we continue checking all possible pairs.

### Implementation

```java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        // Loop through each element in the array as the first element of the pair
        for (int i = 0; i < nums.length; i++) {
            // Loop through each element after the i-th element
            for (int j = i + 1; j <= i + k && j < nums.length; j++) {
                // If the same element is found within k distance, return true
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        // If no such pair is found, return false
        return false;
    }
}
```

### Time Complexity
- **O(n * k)**: This approach involves checking all possible pairs in the array where `n` is the length of the array.

### Space Complexity
- **O(1)**: We are only using a constant amount of extra space.

---

## Sliding Window with HashSet

### Intuition
Instead of checking each pair, we can use a sliding window of size `k` and a HashSet to track the elements within that window. As we slide the window through the array, we can keep checking if the current element already exists in the HashSet. If it does, it means we found a duplicate within the given range.

### Implementation

```java
import java.util.HashSet;

public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashSet<Integer> set = new HashSet<>();
        
        for (int i = 0; i < nums.length; i++) {
            // If set already contains this number, we found a duplicate within k distance
            if (set.contains(nums[i])) {
                return true;
            }
            // Add current number to the set
            set.add(nums[i]);
            // If the size of the set exceeds k, remove the oldest element
            if (set.size() > k) {
                set.remove(nums[i - k]);
            }
        }
        // If no duplicates found, return false
        return false;
    }
}
```

### Time Complexity
- **O(n)**: We iterate through the list once, each operation (add, remove, contains) of HashSet takes O(1) on average.

### Space Complexity
- **O(min(n, k))**: The space used by the HashSet scales with the size of `k`, as it only keeps track of `k` elements at a time.

---

## Sliding Window with HashMap

### Intuition
A small optimization can be done over the HashSet approach by using a HashMap which stores the indices. By keeping track of indices, we can avoid unnecessary otherwise possible duplicates removal. This approach reduces operations to a bare minimum required for ensuring unique elements within the window.

### Implementation

```java
import java.util.HashMap;

public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            // If map already has the number and the difference between indices is <= k
            if (map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) {
                return true;
            }
            // Update the latest index of the current number
            map.put(nums[i], i);
        }
        // If no such pair found, return false
        return false;
    }
}
```

### Time Complexity
- **O(n)**: We traverse the array once, each operation with HashMap is O(1) on average.

### Space Complexity
- **O(n)**: In the worst case, all elements might be unique in the array. Hence, the HashMap can store up to `n` unique elements and their last seen indices.

