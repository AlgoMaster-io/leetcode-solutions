# [Leetcode 260: Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approaches
1. [Brute Force Using Hash Map](#brute-force-using-hash-map)
2. [Optimized Approach Using Bit Manipulation](#optimized-approach-using-bit-manipulation)

### Brute Force Using Hash Map

#### Intuition:
The problem requires us to identify two unique numbers in an array where every other number appears twice. A straightforward way to do this is by using a hash map to count occurrences of each number. After processing the array, any number with a count of one is part of our answer.

#### Code:
```javascript
function singleNumber(nums) {
    // Create a hash map to store the frequency of each number
    const frequencyMap = {};

    // Populate the frequency map
    for (let num of nums) {
        // If the number exists in the map, increment its count
        if (frequencyMap[num]) {
            frequencyMap[num] += 1;
        } else {
            // Otherwise, initialize it with count 1
            frequencyMap[num] = 1;
        }
    }

    // Array to store the result
    const result = [];

    // Iterate over the map to find numbers with frequency 1
    for (let num in frequencyMap) {
        if (frequencyMap[num] === 1) {
            result.push(parseInt(num));
        }
    }

    return result;
}
```

#### Time Complexity:
- **O(n)**: We traverse the array twice, once for counting and once for collecting unique numbers, which are both linear operations.
  
#### Space Complexity:
- **O(n)**: We store each number's frequency, which takes additional space proportional to the size of the input in the worst case.

### Optimized Approach Using Bit Manipulation

#### Intuition:
When XOR-ing numbers, pairs become zero since a number XOR-ed with itself results in zero. Consequently, when you XOR the entire array, the result will be the XOR of the two unique numbers (`a XOR b`). To separate these two numbers, we find a set bit in the XOR result (this bit must differ between `a` and `b`). We can then divide the numbers into two groups based on this bit and perform XOR again on these groups separately to find `a` and `b`.

#### Code:
```javascript
function singleNumber(nums) {
    let xor = 0;

    // XOR all numbers to get a XOR b
    for (let num of nums) {
        xor ^= num;
    }

    // Find the rightmost set bit in xor (a XOR b)
    let diffBit = xor & -xor;

    // Initialize result numbers
    let num1 = 0, num2 = 0;

    // Separate numbers into two groups and XOR within them
    for (let num of nums) {
        if (num & diffBit) {
            num1 ^= num; // XOR group one
        } else {
            num2 ^= num; // XOR group two
        }
    }

    return [num1, num2];
}
```

#### Time Complexity:
- **O(n)**: The array is traversed in linear time twice: once to find the XOR of the entire array and once to segregate and identify the unique numbers.
  
#### Space Complexity:
- **O(1)**: We only use a constant amount of extra space unrelated to the size of the input. 

The bit manipulation approach makes it possible to solve the problem efficiently without additional data structures, leveraging properties of XOR operations to isolate the unique numbers.

