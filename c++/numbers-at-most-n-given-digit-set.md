# [Leetcode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approaches:
- [Approach 1: Brute Force with Backtracking](#approach-1-brute-force-with-backtracking)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

---

## Approach 1: Brute Force with Backtracking

### Intuition:
The idea is to generate all numbers starting from 0 and check if they are less than or equal to N. We will use a recursive backtracking method to construct numbers from the largest available digits from the set.

1. Start building numbers digit by digit from the given set.
2. For each digit level, try every possible digit.
3. If the current number exceeds N, backtrack as all larger numbers would exceed N.
4. If the number formed is valid and less than N, count it.

### Code:
```cpp
class Solution {
public:
    // Helper function for recursive number formation
    void backtrack(string &D, string &current, int pos, int N, int &count) {
        // Convert the current string to an integer
        int num = current.empty() ? 0 : stoi(current);
        
        // If the formed number is greater than N, we terminate this path
        if (num > N) return;
        
        // If the number is positive and less than or equal to N, count it
        if (!current.empty() && num <= N) count++;
        
        // Try to append each digit in D to the current number
        for (auto c : D) {
            current.push_back(c);  // Append the digit
            backtrack(D, current, pos + 1, N, count);  // Recurse further
            current.pop_back();  // Backtrack to consider a different digit
        }
    }
    
    int atMostNGivenDigitSet(vector<string>& D, int N) {
        int count = 0;
        string current = "";
        string digitSet = "";  
        
        // Convert vector of strings to a single string 
        for (auto& d : D) digitSet += d;
        
        backtrack(digitSet, current, 0, N, count);
        return count;
    }
};
```

### Complexity:
- **Time Complexity:** \( O(D^k) \), where \( D \) is the size of the digit set and \( k \) is the number of digits in \( N \). This is because for each digit we can choose any digit from the set.
- **Space Complexity:** \( O(k) \) for the recursive call stack, ignoring the space used to store the results.

---

## Approach 2: Dynamic Programming

### Intuition:
Instead of generating all possible numbers, we can use a more efficient approach using dynamic programming to calculate the count. The core idea is to handle different lengths of numbers systematically.

1. Calculate how many numbers we can form considering numbers of all lengths less than the length of N.
2. For numbers of the same length as N, use DP to compare each digit position from left to right.
3. Utilize the fact that smaller numbers (those with fewer digits) and valid prefixes lead to valid numbers.

### Code:
```cpp
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& D, int N) {
        string NS = to_string(N);
        int K = NS.size();
        int dp[K + 1];
        dp[K] = 1;

        for (int i = K - 1; i >= 0; --i) {
            int Si = NS[i] - '0';
            dp[i] = 0;
            
            // Check all digits in D
            for (auto &d : D) {
                if (d[0] - '0' < Si) {
                    // If d[0] is less than the digit at position i in N
                    dp[i] += pow(D.size(), K - i - 1);
                } else if (d[0] - '0' == Si) {
                    // If d[0] matches the digit at position i in N
                    dp[i] += dp[i + 1];
                }
            }
        }

        // Count numbers with fewer digits than N
        int answer = 0;
        for (int i = 1; i < K; ++i) {
            answer += pow(D.size(), i);
        }

        return answer + dp[0];
    }
};
```

### Complexity:
- **Time Complexity:** \( O(K \cdot D) \), where \( K \) is the number of digits in N and \( D \) is the size of the digit set.
- **Space Complexity:** \( O(K) \) for the dp array.


