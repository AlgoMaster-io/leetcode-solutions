# [Leetcode 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Dynamic Programming with Space Optimization](#approach-3-dynamic-programming-with-space-optimization)

---

## Approach 1: Brute Force

### Intuition
The naive solution attempts to simulate every possible pair of buy and sell transactions. We split all possible operations into two transactions and calculate the maximum profit for each possible combination.

### Explanation
1. For every possible day `i`, consider it as the end of the first transaction.
2. Compute the maximum profit possible from day 0 to `i`.
3. For every day `j` after `i`, compute the maximum profit possible from `j` to the end.
4. Sum the two transactions and update the maximum profit if this sum exceeds the previous max profit.

### Code
```javascript
function maxProfit(prices) {
    let maxProfit = 0;
    let n = prices.length;
    
    for (let i = 0; i < n; i++) {
        let firstTransactionProfit = 0;
        for (let m = 0; m <= i; m++) {
            firstTransactionProfit = Math.max(firstTransactionProfit, prices[i] - prices[m]);
        }
        
        let secondTransactionProfit = 0;
        for (let j = i + 1; j < n; j++) {
            for (let k = j + 1; k < n; k++) {
                secondTransactionProfit = Math.max(secondTransactionProfit, prices[k] - prices[j]);
            }
        }
        
        maxProfit = Math.max(maxProfit, firstTransactionProfit + secondTransactionProfit);
    }
    
    return maxProfit;
}
```

### Time Complexity
- O(n^3), where `n` is the number of days. We use three nested loops to simulate two transactions.

### Space Complexity
- O(1), since no additional space proportional to input size is used.

---

## Approach 2: Dynamic Programming

### Intuition
We can use dynamic programming to store the maximum profit up to each day for one or two transactions, leading to optimized calculation compared to the brute force approach.

### Explanation
- Define two arrays: `leftProfits` and `rightProfits`.
  - `leftProfits[i]` holds the maximum profit achievable from day 0 to day `i`.
  - `rightProfits[i]` holds the maximum profit achievable from day `i` to the last day.
- Pass through the input to fill these arrays.
- Finally, iterate through `leftProfits` and `rightProfits` to determine the maximum profit sum from two transactions.

### Code
```javascript
function maxProfit(prices) {
    if (prices.length <= 1) return 0;

    const n = prices.length;
    const leftProfits = Array(n).fill(0);
    const rightProfits = Array(n).fill(0);

    // Fill leftProfits
    let minPrice = prices[0];
    for (let i = 1; i < n; i++) {
        minPrice = Math.min(minPrice, prices[i]);
        leftProfits[i] = Math.max(leftProfits[i - 1], prices[i] - minPrice);
    }

    // Fill rightProfits
    let maxPrice = prices[n - 1];
    for (let i = n - 2; i >= 0; i--) {
        maxPrice = Math.max(maxPrice, prices[i]);
        rightProfits[i] = Math.max(rightProfits[i + 1], maxPrice - prices[i]);
    }

    // Compute the maximum profit by combining left and right transactions
    let maxProfit = 0;
    for (let i = 0; i < n; i++) {
        maxProfit = Math.max(maxProfit, leftProfits[i] + rightProfits[i]);
    }

    return maxProfit;
}
```

### Time Complexity
- O(n), where `n` is the number of days. We traverse the prices array three times.

### Space Complexity
- O(n), due to the use of `leftProfits` and `rightProfits` arrays.

---

## Approach 3: Dynamic Programming with Space Optimization

### Intuition
We can further optimize the previous solution by only keeping track of variables instead of entire arrays. This reduces the space from O(n) to O(1).

### Explanation
- Use variables instead of arrays to store the maximum profit achievable up to each day from the left and from the right.
- Maintain the rolling profits by updating these variables iteratively.

### Code
```javascript
function maxProfit(prices) {
    let firstBuy = Number.MIN_SAFE_INTEGER;
    let firstSell = 0;
    let secondBuy = Number.MIN_SAFE_INTEGER;
    let secondSell = 0;

    for (let price of prices) {
        firstBuy = Math.max(firstBuy, -price);              // Buy 1st stock
        firstSell = Math.max(firstSell, firstBuy + price);  // Sell 1st stock
        secondBuy = Math.max(secondBuy, firstSell - price); // Buy 2nd stock
        secondSell = Math.max(secondSell, secondBuy + price); // Sell 2nd stock
    }

    return secondSell;
}
```

### Time Complexity
- O(n), single pass through the prices array.

### Space Complexity
- O(1), only a fixed number of variables are used.

Each approach improves upon the previous one in either time or space complexity, providing insight into different strategies for tackling the problem.

