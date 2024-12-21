# [Leetcode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Approach 1: Brute Force using Simulation
The simplest approach is to simulate the process of removing stars and the character preceding each star. We start from the beginning of the string and whenever a star is encountered, we remove the previous character and the star itself.

### Intuition:
- Traverse the string from left to right.
- Use a stack-like approach to keep track of the characters that need not be removed. 
- When encountering a star, pop the last character from the stack as it needs to be removed along with the star itself.
- This mimics removing stars iteratively and helps in achieving the desired string.

### Solution:
```cpp
string removeStars(string s) {
    string result;
    for (char ch : s) {
        if (ch == '*') {
            if (!result.empty()) {
                result.pop_back(); // removes the character before the star
            }
        } else {
            result.push_back(ch); // adds the character to the result
        }
    }
    return result;
}
```

### Time Complexity:
- O(n), where n is the length of the string as each character is processed once.

### Space Complexity:
- O(n), in the worst case, we might store all characters in the result.

---

## Approach 2: Optimized In-place Approach
We can optimize the above approach by avoiding the use of additional space and modifying the string in place. Here, we'll use two pointers: one to iterate over the string and another to keep track of the position in the resultant string.

### Intuition:
- Traverse the string from left to right using a pointer.
- Maintain a variable `j` to mark the index of processed characters in the input string.
- Whenever a character is encountered, overwrite at the `j` position and increment `j`.
- When a '*' is encountered, decrease `j` since it negates the previous character.
- This approach essentially uses the input string space to simulate stack behavior without additional storage.

### Solution:
```cpp
string removeStars(string s) {
    int j = 0; // this will also indicate the size of the result part in string
    for (char ch : s) {
        if (ch == '*') {
            if (j > 0) {
                j--; // negate the last input character
            }
        } else {
            s[j++] = ch; // move character to the result position
        }
    }
    return s.substr(0, j); // return the truncated string up to the filled index
}
```

### Time Complexity:
- O(n), where n is the length of the string, since each character is processed once.

### Space Complexity:
- O(1), as we are performing operations in-place within the input string itself.

---

Each approach provides a trade-off between simplicity and space efficiency while maintaining linear time complexity. The in-place approach makes better use of the input string, avoiding extra memory allocation.

