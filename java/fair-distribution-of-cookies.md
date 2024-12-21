### [Leetcode Problem 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

#### Approaches
- [Approach 1: Backtracking (Brute Force)](#approach-1-backtracking-brute-force)
- [Approach 2: Backtracking with Early Pruning](#approach-2-backtracking-with-early-pruning)

---

### Approach 1: Backtracking (Brute Force)

#### Intuition
The problem involves assigning cookies to children in such a way that the most "unfortunate" child (one with the most cookies) is as fortunate as possible. Given the constraints, a brute force backtracking approach can be applied to attempt all possible distributions of cookies to the children.

#### Approach
1. Utilize a backtracking approach to try distributing cookies in every possible way.
2. Use a recursive function to decide at each step which child should receive the current cookie.
3. After assigning all cookies, calculate the current unfairness by identifying the child with the maximum cookies and update the result.
4. Due to the factorial time complexity of trying every distribution, this approach will be inefficient for larger inputs but demonstrates a straightforward solution.

#### Time Complexity
- **O(k^n)**: For each cookie, there are k children to whom it can be assigned.
  
#### Space Complexity
- **O(k)**: To store the cookie count for each child.

```java
class Solution {
    public int distributeCookies(int[] cookies, int k) {
        // Array where `childBins[i]` indicates the number of cookies given to the i-th child
        int[] childBins = new int[k];
        // Recursive backtracking function to distribute cookies
        return backtrack(cookies, childBins, 0, Integer.MAX_VALUE);
    }

    private int backtrack(int[] cookies, int[] childBins, int index, int minUnfairness) {
        if (index == cookies.length) {
            // Get the maximum number of cookies any one child has, to represent unfairness
            int currentMax = 0;
            for (int cookieCount : childBins) {
                currentMax = Math.max(currentMax, cookieCount);
            }
            // Return the minimal unfairness encountered across all paths
            return Math.min(minUnfairness, currentMax);
        }

        for (int i = 0; i < childBins.length; i++) {
            // Assign cookie `index` to the i-th child and backtrack further
            childBins[i] += cookies[index];
            minUnfairness = backtrack(cookies, childBins, index + 1, minUnfairness);
            // Backtrack: remove the assigned cookie from the i-th child
            childBins[i] -= cookies[index];
        }
        return minUnfairness;
    }
}
```

### Approach 2: Backtracking with Early Pruning

#### Intuition
We can enhance the previous backtracking approach by pruning paths early when they don't promise a better result. This can be done by ensuring that no intermediate state has an unfairness greater than the currently found best solution.

#### Approach
1. Similar to the previous method, but with a check before descending deeper into recursion.
2. Each time a cookie is assigned, check if the current unfairness already exceeds the current best solution.
3. If it does, prune this path (do not continue recursively).
4. This limits unnecessary computation and reduces search space.

#### Time Complexity
- **O(k^n)** in the worst case, but with pruning, often much faster.

#### Space Complexity
- **O(k)**: For storing cookies counts per child.

```java
class Solution {
    public int distributeCookies(int[] cookies, int k) {
        int[] childBins = new int[k];
        // Initial call with a very large unfairness (Infinity) as the starting point
        return backtrack(cookies, childBins, 0, Integer.MAX_VALUE);
    }

    private int backtrack(int[] cookies, int[] childBins, int index, int minUnfairness) {
        if (index == cookies.length) {
            int currentMax = 0;
            for (int cookieCount : childBins) {
                currentMax = Math.max(currentMax, cookieCount);
            }
            return Math.min(minUnfairness, currentMax);
        }

        for (int i = 0; i < childBins.length; i++) {
            childBins[i] += cookies[index];
            
            // Compute current max unfairness after new assignment
            int currentMax = 0;
            for (int cookieCount : childBins) {
                currentMax = Math.max(currentMax, cookieCount);
            }

            if (currentMax < minUnfairness) {
                minUnfairness = backtrack(cookies, childBins, index + 1, minUnfairness);
            }
            childBins[i] -= cookies[index];
        }
        return minUnfairness;
    }
}
```

In summary, each approach provides a progressively efficient way of distributing cookies fairly by leveraging different levels of optimization within the backtracking framework.

