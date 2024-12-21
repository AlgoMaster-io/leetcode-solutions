### [Leetcode Problem 260: Single Number III](https://leetcode.com/problems/single-number-iii/)

**Approaches:**
- [Approach 1: HashSet](#approach-1-hashset)
- [Approach 2: Bit Manipulation](#approach-2-bit-manipulation)

---

#### Approach 1: HashSet

**Intuition:**

The basic approach is to use a hash set to determine which numbers appear only once. When traversing the array, if a number is already in the set, it means it appears twice, so we remove it. After processing all numbers, the set will contain the two numbers that appear only once.

**Steps:**
1. Traverse the array and for each number:
   - If the number is not in the set, add it.
   - If the number is already in the set, remove it.
2. At the end of the traversal, the set will contain exactly two numbers, which are the numbers we are looking for.

```csharp
public int[] SingleNumber(int[] nums) {
    // Use HashSet to keep track of numbers
    var set = new HashSet<int>();

    // Traverse each number in nums
    foreach (int num in nums) {
        // If the number already exists in set, remove it
        if (set.Contains(num)) {
            set.Remove(num);
        } else {
            // Add the number to the set if not present
            set.Add(num);
        }
    }

    // Convert the HashSet to an array and return
    return set.ToArray();
}
```

**Time Complexity:** O(n) - We make a constant time operation for each element in the array.
**Space Complexity:** O(n) - In the worst case, the hash set could grow to contain all elements.

---

#### Approach 2: Bit Manipulation

**Intuition:**

The optimal approach leverages the properties of XOR:
- Any number XOR itself is zero.
- Any number XOR zero is the number itself.
Given that the XOR of two identical numbers is zero, we can XOR all numbers, and the result will be the XOR of the two unique numbers since identical numbers will cancel out.

To isolate the two unique numbers:
1. Use XOR over the entire array to get the XOR of the two unique numbers (let's call it `xor`).
2. Find a bit that is set in `xor` (this means the two unique numbers differ at that bit).
3. Use that bit to partition the numbers into two groups. Each of the unique numbers will be in separate groups.
4. XOR the numbers within each group to find the two unique numbers.

**Steps:**

- XOR all numbers. Result is `xor = x ^ y` where `x` and `y` are the numbers we need to find.
- Find a set bit in `xor` (any differing bit between `x` and `y`).
- Partition numbers into two groups with the set bit condition.
- XOR each group separately to isolate the two numbers.

```csharp
public int[] SingleNumber(int[] nums) {
    // xor will hold the XOR of the two numbers we need to find
    int xor = 0;
    
    // XOR all numbers to get xor
    foreach (int num in nums) {
        xor ^= num;
    }

    // Get a set bit (rightmost set bit) in xor
    // this separates the two numbers
    int setBit = xor & -xor;

    // Partition the numbers into two groups and find the missing number in each group
    int num1 = 0, num2 = 0;
    foreach (int num in nums) {
        if ((num & setBit) == 0) {
            num1 ^= num;  // XOR group 1
        } else {
            num2 ^= num;  // XOR group 2
        }
    }

    // Return both unique numbers
    return new int[] { num1, num2 };
}
```

**Time Complexity:** O(n) - We only go through the array a few times.
**Space Complexity:** O(1) - We use a constant amount of space for variables.

---
By using bit manipulation, we optimize both time and space complexity compared to the hash set approach. This illustrates the power of understanding how operations like XOR can be used to efficiently solve problems where specific pairings or mismatches occur.

