# [260. Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approaches
- [Approach 1: HashMap](#approach-1-hashmap)
- [Approach 2: Bit Manipulation with XOR](#approach-2-bit-manipulation-with-xor)

---

## Approach 1: HashMap

### Intuition:
In this approach, the basic idea is to use a hash map to count the frequency of each number in the list. Since every number except for two appears twice, we can identify the two unique numbers by finding which numbers have a count of one in the hash map.

### Implementation:

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>

std::vector<int> singleNumber(std::vector<int>& nums) {
    std::unordered_map<int, int> frequency;

    // Count the occurrences of each number
    for (int num : nums) {
        frequency[num]++;
    }

    std::vector<int> result;

    // Identify the numbers that appear once
    for (const auto& entry : frequency) {
        if (entry.second == 1) {
            result.push_back(entry.first);
        }
    }

    return result;
}

int main() {
    std::vector<int> nums = {1, 2, 1, 3, 2, 5};
    std::vector<int> result = singleNumber(nums);
    std::cout << "The numbers that appear once are: ";
    for (int num : result) {
        std::cout << num << " ";
    }
    return 0;
}
```

### Time Complexity: 
- O(n), where n is the total number of elements in the vector since we traverse the list twice, once for counting and once for finding the unique elements.

### Space Complexity: 
- O(n) due to the space used to store elements in the hash map.

---

## Approach 2: Bit Manipulation with XOR

### Intuition:
This approach utilizes the properties of XOR. XOR of a number with itself is zero, and XOR of a number with zero is the number itself. By XOR-ing all the numbers, the result will be the XOR of the two unique numbers, as the numbers appearing twice cancel out each other. Then, we can find a set bit in this result to help differentiate the two unique numbers based on this differing bit.

### Detailed Steps:
1. **XOR All Numbers**: XOR all numbers. The result will be the XOR of the two unique numbers.
2. **Find a Differentiation Bit**: Find any bit that is set in the XOR result. This helps in partitioning the numbers into two groups.
3. **Partition and XOR**: Partition the numbers into two groups, based on whether the differentiation bit is set, and XOR them separately to find the two unique numbers.

### Implementation:

```cpp
#include <iostream>
#include <vector>

std::vector<int> singleNumber(std::vector<int>& nums) {
    int xor_all = 0;

    // Perform XOR operation on all numbers
    for (int num : nums) {
        xor_all ^= num;
    }

    // Find rightmost set bit different between two unique numbers
    int rightmost_set_bit = xor_all & -xor_all;

    int num1 = 0, num2 = 0;

    // Divide numbers into two groups and find the unique numbers
    for (int num : nums) {
        if (num & rightmost_set_bit) {
            num1 ^= num;  // XOR for the first group
        } else {
            num2 ^= num;  // XOR for the second group
        }
    }

    return {num1, num2};
}

int main() {
    std::vector<int> nums = {1, 2, 1, 3, 2, 5};
    std::vector<int> result = singleNumber(nums);
    std::cout << "The numbers that appear once are: ";
    for (int num : result) {
        std::cout << num << " ";
    }
    return 0;
}
```

### Time Complexity: 
- O(n), where n is the number of elements in the vector. Each element is processed a constant number of times.

### Space Complexity: 
- O(1), as we are using fixed extra space regardless of input size.

