# [Leetcode 169: Majority Element](https://leetcode.com/problems/majority-element/)

## Approaches

- [Approach 1: Brute Force - Nested Loops](#approach-1-brute-force---nested-loops)
- [Approach 2: HashMap Counting](#approach-2-hashmap-counting)
- [Approach 3: Sorting](#approach-3-sorting)
- [Approach 4: Boyer-Moore Voting Algorithm](#approach-4-boyer-moore-voting-algorithm)

### Approach 1: Brute Force - Nested Loops

**Intuition:**

The basic idea is to count the frequency of each element by comparing it with every other element. If a number appears more than `n/2` times in the array, it is the majority element.

**C# Code:**

```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        int majorityCount = nums.Length / 2;
        
        // Try each number to see if it is the majority
        for(int i = 0; i < nums.Length; i++) {
            int count = 0;
            for(int j = 0; j < nums.Length; j++) {
                if(nums[j] == nums[i]) {
                    count++;
                }
            }
            // If this number is the majority, return it
            if(count > majorityCount) {
                return nums[i];
            }
        }
        
        return -1; // This line should never be reached as a majority element always exists
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n^2), where n is the length of the array. We have two nested loops, each iterating over the array.
- **Space Complexity:** O(1), no extra space used.

---

### Approach 2: HashMap Counting

**Intuition:**

We can improve the brute-force approach by using a hash map to count occurrences. This reduces the time complexity by removing the inner loop.

**C# Code:**

```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        var counts = new Dictionary<int, int>();
        
        // Count each element's occurrences
        foreach(var num in nums) {
            if(counts.ContainsKey(num)) {
                counts[num]++;
            } else {
                counts[num] = 1;
            }
            // Return if we found the majority element
            if(counts[num] > nums.Length / 2) {
                return num;
            }
        }
        
        return -1; // Should not reach here
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the array, as we make a single pass through the array.
- **Space Complexity:** O(n), since the dictionary could potentially hold all elements.

---

### Approach 3: Sorting

**Intuition:**

If the array is sorted, the majority element will occupy the center position because it appears more than `n/2` times.

**C# Code:**

```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        Array.Sort(nums); // Sort the array
        return nums[nums.Length / 2]; // The middle element is the majority one
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n log n), due to the sorting operation.
- **Space Complexity:** O(1) or O(n) depending on the sorting algorithm used.

---

### Approach 4: Boyer-Moore Voting Algorithm

**Intuition:**

This is the most optimal approach in terms of space and time. We maintain a count and a candidate for the majority element. When the count is zero, we pick a new candidate. If the current element is the same as the candidate, we increase the count, otherwise, we decrease it.

**C# Code:**

```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;
        
        // Boyer-Moore Voting Algorithm
        foreach(int num in nums) {
            if(count == 0) {
                candidate = num; // Choose a new candidate
            }
            count += (num == candidate) ? 1 : -1; // Adjust the count
        }
        
        return candidate;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the array, because we make a single pass through the array.
- **Space Complexity:** O(1), since we use only a fixed amount of extra space.

