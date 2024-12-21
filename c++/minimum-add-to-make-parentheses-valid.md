[Leetcode Problem 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approaches:
1. [Using a Stack](#using-a-stack)
2. [Count Open and Close Brackets](#count-open-and-close-brackets)
3. [Single Pass Count](#single-pass-count)

### Approach 1: Using a Stack
The first approach uses a stack to help track unbalanced parentheses.

#### Intuition:
- Each time an opening parenthesis '(' is encountered, it is pushed onto the stack.
- For a closing parenthesis ')':
  - If the stack is not empty (there's an unmatched '('), it means the current ')' can be matched with a '(' from the stack, so pop the stack.
  - If the stack is empty, the ')' is unmatched, requiring an additional '('.
- Finally, the size of the stack will tell us the number of unmatched opening parentheses, and the number of unmatched closing parentheses is counted during the process.

```cpp
int minAddToMakeValid(string s) {
    std::stack<char> st;
    int close_needed = 0; // closed parentheses that need a match
    
    for (char c : s) {
        if (c == '(') {
            // Push the opening bracket onto the stack
            st.push(c);
        } else { // c == ')'
            if (!st.empty()) { 
                // There's an unmatched opening bracket, pair it
                st.pop();
            } else {
                // This closing bracket has no match
                ++close_needed;
            }
        }
    }
    
    // All remaining elements in the stack are unmatched '('
    return close_needed + st.size();
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n) for the stack

### Approach 2: Count Open and Close Brackets
This approach performs similar logic to the stack approach, but with less space overhead.

#### Intuition:
- Maintain two counters: `open_cnt` for unmatched '(' and `close_needed` for unmatched ')'.
- Increment `open_cnt` for '('.
- If a ')' is found:
  - Decrease `open_cnt` if it's greater than 0 (indicating an unmatched '(' is available to pair).
  - If `open_cnt` is 0, increment `close_needed` because we have an extra ')'.

```cpp
int minAddToMakeValid(string s) {
    int open_cnt = 0; // unmatched open parentheses
    int close_needed = 0; // unmatched closed parentheses
    
    for (char c : s) {
        if (c == '(') {
            ++open_cnt; // count open
        } else { // c == ')'
            if (open_cnt > 0) {
                --open_cnt; // matching with an open parenthesis
            } else {
                ++close_needed; // need an open to match this close
            }
        }
    }
    
    // open_cnt is unmatched opens, close_needed is unmatched closes
    return open_cnt + close_needed;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1) (Space is used only for counters, no data structures)

### Approach 3: Single Pass Count
The most optimal and intuitive way to count uses a direct tracking of mismatched brackets.

#### Intuition:
- With counters `balance` and `min_add`:
  - `balance` tracks the net number of '(' minus ')'.
  - Add to `min_add` when a ')' is encountered and `balance` is 0 (need an '(').
- Finally, include any remaining `balance` unmatched '(' after the loop.

```cpp
int minAddToMakeValid(string s) {
    int balance = 0; // Track imbalance: '(' counts as +1, ')' as -1
    int min_add = 0; // Minimum additions needed
    
    for (char c : s) {
        if (c == '(') {
            ++balance;
        } else { // c == ')'
            if (balance == 0) {
                // No open to match with, so we need an extra '('
                ++min_add;
            } else {
                // Pop an open for this close
                --balance;
            }
        }
    }
    
    // Add unmatched opens to the result
    return min_add + balance;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1) (Only integer counters are used, no additional space required)

