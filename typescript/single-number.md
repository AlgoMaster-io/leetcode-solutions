# [Leetcode 136: Single Number](https://leetcode.com/problems/single-number/)

## Approaches
1. [Brute Force: Nested Loop](#approach-1-brute-force-nested-loop)
2. [HashMap to store Frequencies](#approach-2-hashmap-to-store-frequencies)
3. [Bit Manipulation using XOR](#approach-3-bit-manipulation-using-xor)

---

## Approach 1: Brute Force: Nested Loop

### Intuition
In this approach, we will iterate through each element and count its occurrences in the array. If an element is found only once, it is our single number. This approach involves using two nested loops.

### Code
```typescript
function singleNumber(nums: number[]): number | null {
    // Loop through each element
    for (let i = 0; i < nums.length; i++) {
        let count = 0;
        // Check the count of nums[i]
        for (let j= 0; j < nums.length; j++) {
            if (nums[i] === nums[j]) {
                count++;
            }
        }
        // If count is 1, we found our single number
        if (count === 1) {
            return nums[i];
        }
    }
    return null;
}
```

### Complexity
- **Time Complexity:** O(n^2) because of the nested loops.
- **Space Complexity:** O(1) as we are using constant extra space.

---

## Approach 2: HashMap to store Frequencies

### Intuition
Instead of using a nested loop, we can use a HashMap to store the frequency of each element. Once we have the frequency of each element, we can easily find the element that appears only once.

### Code
```typescript
function singleNumber(nums: number[]): number | null {
    const frequencyMap: Map<number, number> = new Map();

    // Loop to fill frequency map
    for (const num of nums) {
        if (frequencyMap.has(num)) {
            frequencyMap.set(num, frequencyMap.get(num)! + 1);
        } else {
            frequencyMap.set(num, 1);
        }
    }

    // Find and return the single number
    for (const [num, count] of frequencyMap.entries()) {
        if (count === 1) {
            return num;
        }
    }
    return null;
}
```

### Complexity
- **Time Complexity:** O(n) where n is the number of elements in the array, as we traverse it twice (once to build the map and once to check frequencies).
- **Space Complexity:** O(n) for storing frequencies in the HashMap.

---

## Approach 3: Bit Manipulation using XOR

### Intuition
This approach leverages the properties of XOR operation:
- A number XORed with itself results in 0.
- A number XORed with 0 results in the number itself.

Thus, XORing all numbers in the array will cancel out all numbers that appear twice, leaving us with the single number.

### Code
```typescript
function singleNumber(nums: number[]): number {
    let result = 0;

    // XOR all numbers together
    nums.forEach(num => {
        result ^= num;
    });

    return result;
}
```

### Complexity
- **Time Complexity:** O(n) since we iterate through the array once.
- **Space Complexity:** O(1) as we use constant extra space.

---

