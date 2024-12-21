# [Leetcode 121: Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Solutions:
- [Approach 1: Brute Force](#approach-1:-brute-force)
- [Approach 2: Single Pass](#approach-2:-single-pass)

## Approach 1: Brute Force

### Intuition:
The brute force approach involves checking every possible pair of buying and selling days and calculating the profit. This can be done using a nested loop where the outer loop represents the buying day and the inner loop represents the selling day. This approach tries all possibilities and selects the one with maximum profit.

### Code:
```cpp
int maxProfit(vector<int>& prices) {
    int maxProfit = 0;
    int n = prices.size();

    // Loop over each day as a potential buying day
    for (int i = 0; i < n; ++i) {
        // Loop over each subsequent day as a potential selling day
        for (int j = i + 1; j < n; ++j) {
            // Calculate profit for this buy-sell pair
            int profit = prices[j] - prices[i];
            // Update maxProfit if this profit is greater
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    return maxProfit;
}
```

### Time Complexity:
- O(n^2), where n is the number of days. This is because for each day, we potentially check every other day to find the selling price.

### Space Complexity:
- O(1), as no extra space is used other than a few variables.

## Approach 2: Single Pass

### Intuition:
Instead of checking every possible pair of days, we can keep track of the minimum price seen so far and compute the potential profit for each day by considering it as a selling day. This way, we ensure that we are buying before or on the selling day, keeping the solution valid.

### Code:
```cpp
int maxProfit(vector<int>& prices) {
    int maxProfit = 0;       // Max profit initialized to zero
    int minPrice = INT_MAX;  // Min price initialized to a large number

    // Traverse the array
    for (int price : prices) {
        // If current price is less than minPrice, update minPrice
        if (price < minPrice) {
            minPrice = price;
        } 
        // Else calculate profit and update maxProfit if needed
        else {
            int profit = price - minPrice;
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    return maxProfit;
}
```

### Time Complexity:
- O(n), where n is the number of days since we perform a constant-time operation for each element in the array.

### Space Complexity:
- O(1), as we are using only a constant amount of extra space irrespective of input size.

