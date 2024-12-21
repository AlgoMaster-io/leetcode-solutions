# [Leetcode 70: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

### Approaches
- [Approach 1: Recursion (Brute Force)](#approach-1-recursion-brute-force)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)
- [Approach 4: Fibonacci Formula (Optimal)](#approach-4-fibonacci-formula-optimal)

## Approach 1: Recursion (Brute Force)

### Intuition:
The problem of climbing stairs can be broken down into subproblems of reaching the current step from the previous two steps. By using a recursive approach, we can consider that from each step, we can either take 1 step or 2 steps to reach the top.

### Code:
```typescript
function climbStairs(n: number): number {
  // Base cases: if there are 0 or 1 steps, the number of ways is 1.
  if (n <= 1) {
      return 1;
  }
  // Recursive call: sum of the ways to climb to the last step from (n-1) and (n-2) steps.
  return climbStairs(n - 1) + climbStairs(n - 2);
}
```

### Time Complexity: 
- O(2^n): Because at each step, we call the function twice, leading to an exponential growth.

### Space Complexity: 
- O(n): The height of the recursion tree.

---

## Approach 2: Recursion with Memoization

### Intuition:
To reduce the repeated calculations made during the recursion, we can use memoization to store results of subproblems and reuse them.

### Code:
```typescript
function climbStairs(n: number): number {
  // Memoization map to store results of subproblems.
  const memo = new Map<number, number>();
  
  // Helper function to perform memoized recursion.
  function climb(n: number): number {
    // Base cases.
    if (n <= 1) return 1;
    
    // If the value has been computed, return it.
    if (memo.has(n)) return memo.get(n)!;
    
    // Compute the value, store it in the memo, and return it.
    const result = climb(n - 1) + climb(n - 2);
    memo.set(n, result);
    return result;
  }
  
  return climb(n);
}
```

### Time Complexity:
- O(n): Each number up to n is computed only once.

### Space Complexity:
- O(n): Due to the memoization array and recursion stack.

---

## Approach 3: Dynamic Programming

### Intuition:
We can iteratively build the solution using a bottom-up approach. This eliminates the need for recursion by storing results of subproblems in an array.

### Code:
```typescript
function climbStairs(n: number): number {
  // If there are 0 or 1 steps, the number of ways is trivially 1.
  if (n <= 1) return 1;
  
  // dp array to store number of ways to reach each step.
  const dp: number[] = new Array(n + 1).fill(0);
  dp[0] = 1;
  dp[1] = 1;
  
  // Fill the dp array with the number of ways to climb to each step.
  for (let i = 2; i <= n; i++) {
      dp[i] = dp[i - 1] + dp[i - 2];
  }
  
  // Return the number of ways to reach the nth step.
  return dp[n];
}
```

### Time Complexity:
- O(n): We compute the number of ways for each step from 0 to n.

### Space Complexity:
- O(n): Storage for the dp array.

---

## Approach 4: Fibonacci Formula (Optimal)

### Intuition:
The problem of climbing stairs is analogous to finding the nth Fibonacci number. The number of ways to reach the nth step is the sum of the ways to reach the (n-1)th and (n-2)th steps. By using a space-optimized Fibonacci calculation, we can compute the result in constant space.

### Code:
```typescript
function climbStairs(n: number): number {
  // Start from the first two known Fibonacci numbers.
  let first = 1, second = 1;
  
  // Compute the Fibonacci numbers iteratively and keep updating the previous two.
  for (let i = 2; i <= n; i++) {
      const temp = first + second;
      first = second;
      second = temp;
  }
  
  // Return the nth Fibonacci number, which is stored in 'second'.
  return second;
}
```

### Time Complexity:
- O(n): A single pass over steps.

### Space Complexity:
- O(1): Only two extra variables are used for the calculation.

