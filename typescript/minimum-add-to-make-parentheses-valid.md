# [921. Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approaches
1. [Using Stack](#using-stack)
2. [Counting Open and Close Parentheses](#counting-open-and-close-parentheses)

---

## Using Stack

**Intuition:**

A common approach for balance-checking problems involving parentheses is to use a stack. By pushing open parentheses onto the stack and popping them when we encounter close parentheses, we can easily check for validity. In this problem, instead of simply checking if the parentheses are valid, we need to count how many characters are required to make them valid. Thus, we'll use a stack to track unmatched open parentheses and a counter for unmatched close parentheses.

**Approach:**
1. Initialize a stack to keep track of unmatched open parentheses `'('`.
2. Initialize a counter `close_needed` to count unmatched close parentheses `')'`.
3. Iterate through each character in the string:
   - If the character is `'('`, push it onto the stack.
   - If the character is `')'`, check:
     - If the stack is not empty, pop an element from the stack.
     - If the stack is empty, increment the `close_needed` counter.
4. The result will be the size of the stack plus the `close_needed` counter, which represents unmatched open and unmatched close parentheses, respectively.

**Code:**

```typescript
function minAddToMakeValid(s: string): number {
    // Stack to track unmatched '('
    const stack: string[] = [];
    // Counter for unmatched ')'
    let closeNeeded = 0;

    for (const char of s) {
        if (char === '(') {
            // Push open parentheses onto stack
            stack.push(char);
        } else {
            if (stack.length > 0) {
                // Match found: pop from stack
                stack.pop();
            } else {
                // Unmatched ')', increment counter
                closeNeeded++;
            }
        }
    }

    // Remaining items in stack are unmatched '('
    // closeNeeded is unmatched ')'
    return stack.length + closeNeeded;
}
```

**Time Complexity:** O(n), where n is the length of the string, as we traverse the string once.

**Space Complexity:** O(n), for the stack storage in the worst case scenario.

---

## Counting Open and Close Parentheses

**Intuition:**

An optimized approach involves counting the number of unmatched open `(` and close `)` parentheses without using extra space for a stack. We keep track of the balance of parentheses as we traverse the string.

**Approach:**
1. Initialize `open_needed` to count unmatched open parentheses `'('`.
2. Initialize `close_needed` to count unmatched close parentheses `')'`.
3. Traverse through each character in the string:
   - If the character is `'('`, increment `open_needed`.
   - If the character is `')'`, check:
     - If `open_needed` is greater than 0, decrement it, as we have a matched pair.
     - If `open_needed` is 0, increment `close_needed`.
4. The result is the sum of `open_needed` and `close_needed`, which gives the number of unmatched open and close parentheses.

**Code:**

```typescript
function minAddToMakeValid(s: string): number {
    let openNeeded = 0;  // Counter for unmatched '('
    let closeNeeded = 0; // Counter for unmatched ')'

    for (const char of s) {
        if (char === '(') {
            openNeeded++;  // Need another ')' to balance this
        } else {
            if (openNeeded > 0) {
                openNeeded--; // Found matching '(' for this ')'
            } else {
                closeNeeded++; // Unmatched ')', increase closeNeeded
            }
        }
    }

    // Total corrections required are unmatched '(' + unmatched ')'
    return openNeeded + closeNeeded;
}
```

**Time Complexity:** O(n), where n is the length of the string, as we traverse the string once.

**Space Complexity:** O(1), as we are using only constant extra space for counters.

