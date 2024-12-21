# [Leetcode 1696: Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approaches
- [Approach 1: Basic Dynamic Programming](#approach-1-basic-dynamic-programming)
- [Approach 2: Dynamic Programming with Monotonic Queue](#approach-2-dynamic-programming-with-monotonic-queue)

---

## Approach 1: Basic Dynamic Programming

### Intuition

In the Jump Game VI problem, we need to find the maximum score possible to reach the last index. The straightforward way is to utilize Dynamic Programming (DP) where we store the maximum obtainable score up to each index. We calculate this by iterating through each index and exploring all possible jumps backward within range `k`.

This approach, however, might not be efficient as it has to evaluate each jump length individually within bounds, leading to higher time complexities.

### Implementation

```csharp
public int MaxResult(int[] nums, int k) 
{
    int n = nums.Length;
    int[] dp = new int[n];
    dp[0] = nums[0];

    for (int i = 1; i < n; i++) 
    {
        int maxScore = int.MinValue;  // Initialize maxScore to smallest possible value
        for (int j = i - 1; j >= Math.Max(0, i - k); j--) 
        {
            maxScore = Math.Max(maxScore, dp[j]);  // Find max score in range
        }
        dp[i] = maxScore + nums[i];  // Current max score is max score in range + current value
    }
    return dp[n - 1];  // Return max score to reach the last index
}
```

### Time Complexity

- **Time Complexity**: \(O(n \cdot k)\), where \(n\) is the length of the array. We have to iterate through each index and look backwards up to \(k\) steps.
- **Space Complexity**: \(O(n)\), where \(n\) is the length of the `dp` array used.

---

## Approach 2: Dynamic Programming with Monotonic Queue

### Intuition

To optimize the previous version, we can use a Monotonic Queue to keep track of the indices of the array that can contribute to the maximum sum in an efficient way. This approach helps in reducing the time complexity significantly by maintaining maximum elements in a window using deque operations.

The idea is to maintain a decreasing deque of indices based on the values in `dp` so the first element is always the maximum possible jump to reach the current index. By doing this, for each index, we easily update `dp[i]` to be `nums[i]` plus `dp[deque.front()]`, the maximum in that feasible jump range, and then update the deque accordingly.

### Implementation

```csharp
public int MaxResult(int[] nums, int k) 
{
    int n = nums.Length;
    int[] dp = new int[n];
    dp[0] = nums[0];
    LinkedList<int> deque = new LinkedList<int>();
    deque.AddLast(0);

    for (int i = 1; i < n; i++) 
    {
        // The first element of deque is always in the range
        dp[i] = nums[i] + dp[deque.First.Value];

        // Maintain the deque with indices of the max dp value in range
        while (deque.Count > 0 && dp[i] >= dp[deque.Last.Value])
            deque.RemoveLast();

        deque.AddLast(i);

        // Remove indices that are out of range k
        if (deque.First.Value <= i - k)
            deque.RemoveFirst();
    }
    return dp[n - 1];
}
```

### Time Complexity

- **Time Complexity**: \(O(n)\), where \(n\) is the length of the array. We iterate over each element once, and each element is added and removed from the deque at most once.
- **Space Complexity**: \(O(n)\), due to the space used by the DP array and the deque that can hold at most `k` elements at any time.

This solution provides a significant improvement over the basic one by efficiently managing the window of feasible jumps using a deque, minimizing unnecessary calculations.

