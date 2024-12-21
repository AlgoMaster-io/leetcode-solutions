# [Leetcode 169: Majority Element](https://leetcode.com/problems/majority-element/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: HashMap](#approach-2-hashmap)
- [Approach 3: Sorting](#approach-3-sorting)
- [Approach 4: Boyer-Moore Voting Algorithm](#approach-4-boyer-moore-voting-algorithm)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach involves checking each element and counting its occurrences in the array. If the count of a number is greater than half of the array length, it is the majority element.

### Code:
```java
public int majorityElement(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        int count = 0;
        // Count occurrences of nums[i]
        for (int j = 0; j < n; j++) {
            if (nums[j] == nums[i]) {
                count++;
            }
        }
        // If count exceed n/2, nums[i] is the majority element
        if (count > n / 2) {
            return nums[i];
        }
    }
    return -1; // Should never be reached if majority element assumption holds
}
```

### Time Complexity:
- O(n^2) because for each element, we are iterating through the list to count occurrences.

### Space Complexity:
- O(1) because no extra space is used aside from variables.

---

## Approach 2: HashMap

### Intuition:
We can use a HashMap to store the frequency of each element. Traverse the array, and increment the counter for each element encountered. The element with a frequency greater than `n/2` will be the majority element.

### Code:
```java
import java.util.HashMap;

public int majorityElement(int[] nums) {
    HashMap<Integer, Integer> countMap = new HashMap<>();
    int n = nums.length;
    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        // If an element's count exceeds n/2, return it
        if (countMap.get(num) > n / 2) {
            return num;
        }
    }
    return -1; // Shouldn't reach here if input is valid
}
```

### Time Complexity:
- O(n) because we iterate over the array once.

### Space Complexity:
- O(n) because we might store all elements in the map in the worst case.

---

## Approach 3: Sorting

### Intuition:
If the array is sorted, the majority element must be located at the middle index. As there are more occurrences of the majority element than any other element, it consistently dominates around the `n/2` index.

### Code:
```java
import java.util.Arrays;

public int majorityElement(int[] nums) {
    Arrays.sort(nums);
    return nums[nums.length / 2];
}
```

### Time Complexity:
- O(n log n) due to sorting.

### Space Complexity:
- O(1) when using in-place sorting (ignoring input space).

---

## Approach 4: Boyer-Moore Voting Algorithm

### Intuition:
The Boyer-Moore Voting Algorithm efficiently finds the majority element in linear time. It maintains a candidate and a counter. It increases the counter for the current candidate on matches while switching the candidate when the counter hits zero.

### Code:
```java
public int majorityElement(int[] nums) {
    int candidate = nums[0];
    int count = 0;

    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += (num == candidate) ? 1 : -1;
    }
    return candidate;
}
```

### Time Complexity:
- O(n) since it passes through the array once.

### Space Complexity:
- O(1) since only a few additional variables are used.

