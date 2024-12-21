# [Leetcode 260: Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approaches
- [Naive Approach: HashMap](#naive-approach-hashmap)
- [Optimized Approach: Bit Manipulation](#optimized-approach-bit-manipulation)

### Naive Approach: HashMap

**Intuition:**

The naive approach to solve `Single Number III` is by using a HashMap (or HashTable) to store the frequency of each number in the input array. The numbers that appear exactly once are our answer.

**Steps:**

1. Traverse the array and populate the HashMap with the frequency of each number.
2. Traverse the HashMap and collect numbers that have a frequency of 1.

**Complexity:**

- **Time Complexity:** O(n), where n is the length of the input array. This is because we iterate over the array twice (once to fill the hashmap and once to collect single numbers).
- **Space Complexity:** O(n), due to the space used by the hashmap to store frequencies.

```typescript
function singleNumber(nums: number[]): number[] {
    const frequencyMap: Map<number, number> = new Map();
    // First pass: populate the frequency map
    for (let num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }
    
    // Second pass: collect numbers that appear exactly once
    const result: number[] = [];
    for (let [key, value] of frequencyMap.entries()) {
        if (value === 1) {
            result.push(key);
        }
    }
    
    return result;
}
```

### Optimized Approach: Bit Manipulation

**Intuition:**

The optimized solution involves using bit manipulation and the XOR operation. When we XOR all the numbers, the result is the XOR of the two unique numbers because pairs cancel each other out. We can then isolate the rightmost set bit (difference) to differentiate between the two unique numbers.

**Steps:**

1. XOR all the numbers in the array. Let's call this result `xor_all`. 
2. The bits of `xor_all` represent different bits between the two single numbers. We can find any set bit (one in the result).
3. Use the set bit to partition the array into two groups. XOR the numbers in each group separately.
4. The result from each group will be the two unique numbers.

**Complexity:**

- **Time Complexity:** O(n), where n is the length of the input array. We only iterate over the array in linear time.
- **Space Complexity:** O(1), as we are using a constant amount of extra space.

```typescript
function singleNumber(nums: number[]): number[] {
    let xorAll = 0;
    // First pass: XOR all numbers to get XOR of two unique numbers
    for (let num of nums) {
        xorAll ^= num;
    }
    
    // Get rightmost set bit in xorAll
    const diffBit = xorAll & -xorAll;
    
    let num1 = 0, num2 = 0;
    
    // Second pass: partition the array based on the set bit position and XOR within each group to isolate the unique number
    for (let num of nums) {
        if (num & diffBit) {
            num1 ^= num;
        } else {
            num2 ^= num;
        }
    }
    
    return [num1, num2];
}
```

This optimized approach uses the properties of XOR and bit manipulation to find the two unique numbers efficiently.

