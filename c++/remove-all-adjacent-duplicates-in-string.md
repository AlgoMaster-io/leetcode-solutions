# [Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
- [Approach 1: Iterative Removal with String Manipulation](#approach-1-iterative-removal-with-string-manipulation)
- [Approach 2: Using Stack](#approach-2-using-stack)

## Approach 1: Iterative Removal with String Manipulation

### Intuition:
The most straightforward approach to solving this problem is to iterate over the string and continuously remove pairs of adjacent duplicates until no more such pairs are found. This is a brute force approach but helps in understanding the problem.

### Detailed Steps:
1. Traverse the string.
2. Check each character and its successor to see if they are identical.
3. If a pair of duplicates is found, remove those two characters from the string.
4. Reset the traversal to start over since the removal might have created new adjacent duplicates.
5. Repeat the process until no duplicates are found.

### Code:
```cpp
#include <iostream>
#include <string>

std::string removeDuplicates(std::string s) {
    bool found;
    do {
        found = false;
        for (int i = 0; i < s.length() - 1; ++i) {
            if (s[i] == s[i + 1]) {
                s.erase(i, 2); // Remove the pair
                found = true; // Mark that a removal happened
                break; // Need to start over due to change in the string
            }
        }
    } while (found);

    return s;
}

int main() {
    std::string s = "abbaca";
    std::cout << removeDuplicates(s) << std::endl; // Output: "ca"
    return 0;
}
```

### Complexity:
- **Time Complexity**: O(n^2) in the worst case, due to repeated removal operations and iterations over the string.
- **Space Complexity**: O(1), using a constant amount of extra space.

## Approach 2: Using Stack

### Intuition:
A more efficient approach is to use a stack data structure to check and manage characters. This allows us to handle adjacent duplicates without rescanning the string multiple times, improving efficiency.

### Detailed Steps:
1. Initialize an empty stack to hold characters.
2. Iterate over each character in the string:
   - If the stack is not empty and the top of the stack is equal to the current character, it means they are duplicates, so pop the stack.
   - Otherwise, push the current character onto the stack.
3. After processing the entire string, the stack will contain the final characters without any adjacent duplicates.
4. Construct the resulting string from the characters in the stack.

### Code:
```cpp
#include <iostream>
#include <vector>

std::string removeDuplicates(std::string s) {
    std::vector<char> stack; // Using vector to simulate a stack
    
    for (char c : s) {
        if (!stack.empty() && stack.back() == c) {
            stack.pop_back(); // Remove the top element if it's a duplicate
        } else {
            stack.push_back(c); // Push the current character onto the stack
        }
    }
    
    return std::string(stack.begin(), stack.end());
}

int main() {
    std::string s = "abbaca";
    std::cout << removeDuplicates(s) << std::endl; // Output: "ca"
    return 0;
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the length of the string, as each character is pushed and popped from the stack at most once.
- **Space Complexity**: O(n) in the worst case, which happens when all characters are unique or not removable duplicates.

By using the stack-based approach, we achieve a linear time complexity solution, which is optimal for this problem.

