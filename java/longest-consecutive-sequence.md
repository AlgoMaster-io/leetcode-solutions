# [Leetcode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: HashSet and Intelligent Search](#approach-2-hashset-and-intelligent-search)
- [Approach 3: Union-Find](#approach-3-union-find)

### Approach 1: Sorting

**Intuition:**

The simplest way to find the longest consecutive sequence is to first sort the array. Once sorted, a consecutive sequence will appear as continuous numbers in the array. We can then iterate through the sorted array to count the lengths of consecutive sequences and return the length of the longest one.

1. **Sort the Array**: Sorting the array costs O(n log n) time.
2. **Initialize counters**: Use `currentStreak` to keep track of the streak length, and `longestStreak` to store the maximum length.
3. **Iterate through the Array**:
   - Continue the streak if consecutive numbers are found.
   - Reset `currentStreak` if a gap is found.
4. **Return the longest streak**.

```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    Arrays.sort(nums);

    int longestStreak = 1;
    int currentStreak = 1;

    for (int i = 1; i < nums.length; i++) {
        // if the current element is identical to the previous,
        // just continue through the iteration
        if (nums[i] == nums[i - 1]) {
            continue;
        }
        // check for consecutive sequence
        if (nums[i] == nums[i - 1] + 1) {
            currentStreak += 1;
        } else {
            longestStreak = Math.max(longestStreak, currentStreak);
            currentStreak = 1;
        }
    }
    return Math.max(longestStreak, currentStreak);
}
```

**Complexities:**
- **Time complexity**: O(n log n), where n is the length of the array due to sorting.
- **Space complexity**: O(1), apart from the input array storage.

### Approach 2: HashSet and Intelligent Search

**Intuition:**

We can leverage a `HashSet` to eliminate duplicates and achieve O(n) complexity for finding consecutive sequences by only attempting to build from numbers that are not part of any smaller sequence.

1. **Add numbers to HashSet**: This allows constant-time lookups.
2. **Iterate over the Set**:
   - For each number, check if it is the start of a sequence.
   - Calculate the length of that sequence by incrementing and checking the presence of subsequent numbers.
3. **Track the maximum sequence length**.

```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    Set<Integer> numSet = new HashSet<>();
    for (int num : nums) {
        numSet.add(num);
    }

    int longestStreak = 0;

    for (int num : numSet) {
        // Check if num is the beginning of a sequence
        if (!numSet.contains(num - 1)) {
            int currentNum = num;
            int currentStreak = 1;

            // Increment currentNum to count the length of sequence
            while (numSet.contains(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }
    return longestStreak;
}
```

**Complexities:**
- **Time complexity**: O(n), where n is the length of the array due to one pass through the elements and constant time lookups.
- **Space complexity**: O(n), because of the space used by the HashSet.

### Approach 3: Union-Find

**Intuition:**

We can use the union-find data structure to connect numbers that are part of consecutive sequences. Each element is a node, and paths connect consecutive numbers. Using path compression and union, we can efficiently find the size of the largest connected component, which corresponds to the longest sequence.

1. **Initialization**:
   - Treat each element in the array as its own set.
2. **Union by Rank**:
   - Connect consecutive numbers.
3. **Find the maximum component size**.

```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    Map<Integer, Integer> parent = new HashMap<>();
    for (int num : nums) {
        parent.put(num, num);
    }

    Map<Integer, Integer> rank = new HashMap<>();
    for (int num : nums) {
        rank.put(num, 1);
    }

    for (int num : nums) {
        if (parent.containsKey(num - 1)) {
            union(num, num - 1, parent, rank);
        }
        if (parent.containsKey(num + 1)) {
            union(num, num + 1, parent, rank);
        }
    }

    int maxStreak = 0;
    for (int num : parent.keySet()) {
        if (find(num, parent) == num) {
            maxStreak = Math.max(maxStreak, rank.get(num));
        }
    }

    return maxStreak;
}

private int find(int num, Map<Integer, Integer> parent) {
    if (parent.get(num) != num) {
        parent.put(num, find(parent.get(num), parent));
    }
    return parent.get(num);
}

private void union(int num1, int num2, Map<Integer, Integer> parent, Map<Integer, Integer> rank) {
    int root1 = find(num1, parent);
    int root2 = find(num2, parent);

    if (root1 != root2) {
        if (rank.get(root1) > rank.get(root2)) {
            parent.put(root2, root1);
            rank.put(root1, rank.get(root1) + rank.get(root2));
        } else {
            parent.put(root1, root2);
            rank.put(root2, rank.get(root1) + rank.get(root2));
        }
    }
}
```

**Complexities:**
- **Time complexity**: Approximately O(n), taking into account the inverse Ackermann function in the union-find operations.
- **Space complexity**: O(n), for the storage in the parent and rank maps.

