## [Leetcode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

### Approaches:
1. [Basic Brute-Force Approach](#basic-brute-force-approach)
2. [Sorting Approach](#sorting-approach)
3. [Optimized HashSet Approach](#optimized-hashset-approach)

---

### Basic Brute-Force Approach

**Intuition:**

In the brute-force approach, for every number in the array, we try to expand both forward and backward to find the longest sequence possible for that start. This ensures we check all potential sequences but is inefficient due to repeated calculations.

```javascript
function longestConsecutive(nums) {
    if (nums.length === 0) return 0;

    let longestStreak = 0;

    for (let num of nums) {
        let currentNum = num;
        let currentStreak = 1;

        // Try to count the sequence expanding forward
        while (nums.includes(currentNum + 1)) {
            currentNum += 1;
            currentStreak += 1;
        }

        longestStreak = Math.max(longestStreak, currentStreak);
    }

    return longestStreak;
}

// Time complexity: O(n^3) in the worst case due to includes method being O(n)
// Space complexity: O(1)
```

### Sorting Approach

**Intuition:**

By sorting the array, consecutive numbers appear next to each other. We can then iterate through the sorted array to find the longest sequence of consecutively increasing integers. This is simpler and more efficient than the brute-force method.

```javascript
function longestConsecutive(nums) {
    if (nums.length === 0) return 0;

    nums.sort((a, b) => a - b);

    let longestStreak = 1;
    let currentStreak = 1;

    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[i - 1]) {
            if (nums[i] === nums[i - 1] + 1) {
                currentStreak += 1;
            } else {
                longestStreak = Math.max(longestStreak, currentStreak);
                currentStreak = 1;
            }
        }
    }

    return Math.max(longestStreak, currentStreak);
}

// Time complexity: O(n log n) due to sorting
// Space complexity: O(1) or O(n) if considering the storage needed for sorting
```

### Optimized HashSet Approach

**Intuition:**

The optimal solution can be achieved using a HashSet. By placing each number in a set, we can quickly check the presence of a number. The key observation is that numbers at the start of a sequence do not have a predecessor in the set. Thus, for each num, if num - 1 is not in the set, we can start counting the length of the sequence from num.

```javascript
function longestConsecutive(nums) {
    let numSet = new Set(nums);
    let longestStreak = 0;

    // Iterate through each number only initiating calculation on potential sequence starters
    for (let num of numSet) {
        // Only check for numbers that might be the start of a sequence
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentStreak = 1;

            // Expand the sequence as long as the next consecutive number exists
            while (numSet.has(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            // Update the longest streak found
            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }

    return longestStreak;
}

// Time complexity: O(n) since each number and related sequence is processed in constant time due to HashSet
// Space complexity: O(n) to store numbers in the HashSet
```

Each subsequent approach improves on performance by reducing unnecessary checks, thus achieving an efficient balance between computational efficiency and ease of implementation.

