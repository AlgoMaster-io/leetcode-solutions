## [Leetcode 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

#### Approaches:
- [Approach 1: Stack](#approach-1-stack)
- [Approach 2: Counting Mismatches](#approach-2-counting-mismatches)

---

### Approach 1: Stack

**Intuition:**

- The problem requires us to determine the minimum number of parentheses we must add to make the sequence valid.
- A stack is a natural fit for problems involving matching parentheses because it allows us to conveniently "remember" opening parentheses and match them with closing ones.
- As we traverse the string, we will push each '(' onto the stack.
- When encountering a ')', we will pop an element from the stack if possible (indicating a pair match); otherwise, we will count how many unmatched closing brackets ')' we encounter.
- At the end of the traversal, the stack may hold unmatched '(' which need closing, and our counter will have counted unmatched ')' which need opening parentheses.
  
**Algorithm:**

1. Initialize an empty stack.
2. Initialize a counter for unmatched closing brackets.
3. Iterate over each character in the string.
   - If the character is '(', push it onto the stack.
   - If the character is ')', check:
     - If the stack is not empty, pop from the stack.
     - If the stack is empty, increment the counter for unmatched ')'.
4. At the end, the stack's size will tell us how many unmatched '(' there are. Add this to the counter for unmatched ')' to get the total number of additions needed.

```python
def minAddToMakeValid(s: str) -> int:
    stack = []
    unmatched_closing = 0
    
    for char in s:
        if char == '(':
            stack.append(char)
        elif char == ')':
            if stack:
                stack.pop()  # Match found
            else:
                unmatched_closing += 1  # No match for ')'
    
    unmatched_opening = len(stack)
    
    return unmatched_opening + unmatched_closing
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n)\), where \(n\) is the length of the string. We make one pass through the string.
- **Space Complexity:** \(O(n)\), in the worst case all characters are '(' requiring storage in the stack.

---

### Approach 2: Counting Mismatches

**Intuition:**

- This approach streamlines the stack method by using two simple counters to directly calculate the number of unbalanced parentheses.
- Instead of using a stack, maintain a balance counter that increases for '(' and decreases for ')'.
- If at any point the balance goes negative (more ')' than '(' so far), it implies there's an unmatched ')'. We should increase our mismatch counter and reset balance.

**Algorithm:**

1. Initialize a balance counter and unmatched counter to zero.
2. Traverse the string character by character:
   - If it is an '(', increment the balance.
   - If it is a ')', decrement the balance. If the balance becomes negative (indicating more ')' than '(' seen so far), increment the unmatched ')' counter and reset the balance.
3. After the loop, the balance will give the unpaired '(' needed.
4. The result is the sum of unmatched ')', and the resulting balance which indicates unmatched '('.

```python
def minAddToMakeValid(s: str) -> int:
    balance = 0
    unmatched_closing = 0
    
    for char in s:
        if char == '(':
            balance += 1
        else:  # char == ')'
            balance -= 1
        
        # Whenever balance goes negative, we have an unmatched closing bracket
        if balance < 0:
            unmatched_closing += 1
            balance = 0  # Reset balance to 0 after accounting for unmatched
    
    return unmatched_closing + balance
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n)\), where \(n\) is the length of the string.
- **Space Complexity:** \(O(1)\), as we only use a few counters and no additional data structures. 

This counting approach provides a cleaner, more efficient solution with lower space usage compared to the stack method.

