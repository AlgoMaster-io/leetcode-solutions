# [Basic Calculator II - LeetCode](https://leetcode.com/problems/basic-calculator-ii/)

## Approaches:

- [Approach 1: Two Stacks Method](#approach-1-two-stacks-method)
- [Approach 2: One Stack Method with Operand Optimization](#approach-2-one-stack-method-with-operand-optimization)
- [Approach 3: In-Place Calculation Using Single Pass and Single Stack](#approach-3-in-place-calculation-using-single-pass-and-single-stack)

---

## Approach 1: Two Stacks Method

### Intuition:
The problem requires us to evaluate a string containing a mathematical expression with `+`, `-`, `*`, and `/`. A straightforward approach is to use two stacks: one for numbers and another for operators. This is similar to the standard method used for evaluating expressions.

### Steps:
1. Use two stacks: one for operands (numbers) and one for operators.
2. Traverse each character in the string:
   - If it's a digit, build the number.
   - If it's an operator, push the current number to the operands stack, then push the operator to the operators stack.
   - Evaluate whenever necessary (`*` and `/` have higher precedence).
3. Pop remaining operators and operands to calculate the result using a simple post-fix evaluation.
4. Return the result.

### Java Code:
```java
import java.util.Stack;

public class BasicCalculatorII {
    public int calculate(String s) {
        if (s == null || s.length() == 0) return 0;

        Stack<Integer> operands = new Stack<>();
        Stack<Character> operators = new Stack<>();

        char[] chars = s.toCharArray();
        int num = 0;
        char lastSign = '+';

        for (int i = 0; i < chars.length; i++) {
            char c = chars[i];
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }
            if (!Character.isDigit(c) && c != ' ' || i == chars.length - 1) {
                if (lastSign == '+') {
                    operands.push(num);
                } else if (lastSign == '-') {
                    operands.push(-num);
                } else {
                    int top = operands.pop();
                    if (lastSign == '*') {
                        operands.push(top * num);
                    } else if (lastSign == '/') {
                        operands.push(top / num);
                    }
                }
                lastSign = c;
                num = 0;
            }
        }

        int result = 0;
        for (int number : operands) {
            result += number;
        }
        return result;
    }
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the length of the string.
- **Space Complexity**: O(n), due to the space used by the stacks.

---

## Approach 2: One Stack Method with Operand Optimization

### Intuition:
By recognizing the fact that `*` and `/` operations are executed immediately and have higher precedence, we can simplify our approach by using only one stack.
We can optimize by directly evaluating the immediate `*` and `/` results on the numbers residing in the stack.

### Steps:
1. Initialize one stack to store evaluated values.
2. Traverse the string similarly and calculate results on the fly based on operator priorities.
3. Append intermediate results computed by `*` and `/` into the stack.
4. Sum up the stack to get the final result.

### Java Code:
```java
import java.util.Stack;

public class BasicCalculatorII {
    public int calculate(String s) {
        if (s == null || s.length() == 0) return 0;

        Stack<Integer> stack = new Stack<>();
        int num = 0;
        char lastSign = '+';
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                if (lastSign == '+') {
                    stack.push(num);
                } else if (lastSign == '-') {
                    stack.push(-num);
                } else if (lastSign == '*') {
                    stack.push(stack.pop() * num);
                } else if (lastSign == '/') {
                    stack.push(stack.pop() / num);
                }
                lastSign = c;
                num = 0;
            }
        }
        
        int result = 0;
        for (int number : stack) {
            result += number;
        }
        return result;
    }
}
```

### Complexity:
- **Time Complexity**: O(n)
- **Space Complexity**: O(n)

---

## Approach 3: In-Place Calculation Using Single Pass and Single Stack

### Intuition:
We can make this more efficient by minimizing stack use to only necessary intermediate results. We can modify the processed result in place using a single pass through the string.
By evaluating high-priority operations as soon as possible, only results of addition/subtraction need to be stored.

### Steps:
1. Use a single stack to store previous evaluated results and only nums needed for adjusting the result from unfinished operations when needed.
2. Track running total adjusted with prior operations.
3. Evaluate high-precedence operations (`*`, `/`) immediately and push intermediate results into the stack.
4. Perform addition and subtraction on accumulated results from the stack at the end.

### Java Code:
```java
public class BasicCalculatorII {
    public int calculate(String s) {
        int num = 0;
        char lastSign = '+';
        int lastNumber = 0;
        int result = 0;

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                if (lastSign == '+') {
                    result += lastNumber;
                    lastNumber = num;
                } else if (lastSign == '-') {
                    result += lastNumber;
                    lastNumber = -num;
                } else if (lastSign == '*') {
                    lastNumber = lastNumber * num;
                } else if (lastSign == '/') {
                    lastNumber = lastNumber / num;
                }
                lastSign = c;
                num = 0;
            }
        }
        
        result += lastNumber;
        return result;
    }
}
```

### Complexity:
- **Time Complexity**: O(n)
- **Space Complexity**: O(1), due to constant space use.

