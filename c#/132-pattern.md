# [Leetcode 456: 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Solutions

- [Brute Force Approach](#brute-force-approach)
- [Optimal Solution Using Stack](#optimal-solution-using-stack)

### Brute Force Approach

#### Intuition
The brute force solution involves checking every possible subsequence of length three for each triplet `(i, j, k)` with conditions `i < j < k` and `nums[i] < nums[k] < nums[j]`. This method essentially checks all combinations of triplets to see if they form the "132" pattern.

#### Approach
1. Use three nested loops to iterate over each possible triplet `(i, j, k)`.
2. For each triplet, check if they satisfy the "132" pattern: `nums[i] < nums[k] < nums[j]`.
3. If we find such a triplet, return `true`.
4. If we finish all iterations without finding such a triplet, return `false`.

#### Complexity
- **Time Complexity**: O(n^3), where n is the length of the input array.
- **Space Complexity**: O(1), as we use only a constant amount of extra space.

```csharp
public bool Find132patternBruteForce(int[] nums) 
{
    int n = nums.Length;
    for (int i = 0; i < n; i++) 
    {
        for (int j = i + 1; j < n; j++) 
        {
            for (int k = j + 1; k < n; k++) 
            {
                // Check for the "132" pattern
                if (nums[i] < nums[k] && nums[k] < nums[j]) 
                {
                    return true;
                }
            }
        }
    }
    return false;
}
```

### Optimal Solution Using Stack

#### Intuition
This solution involves using a stack to efficiently find the "132" pattern. The idea is to iterate backward through the array and keep track of potential third elements (`nums[k]` in the pattern) using a stack. We maintain a variable to track the potential `nums[k]` as we traverse backwards. If we find a triplet satisfying the 132 conditions, we return true.

#### Approach
1. Initialize a stack to keep potential third elements (`nums[k]`).
2. Start traversing from the end of the array towards the beginning.
3. Keep track of the maximum `nums[k]` found (`third`), initially set to the minimum integer value.
4. For each element `nums[j]`, check if it can potentially be the middle element in the "132" pattern with `third` as `nums[k]`.
5. If `nums[j]` is less than `third`, we've found a valid "132" pattern.
6. If not, push `nums[j]` to the stack, updating `third` if necessary when elements are popped.
7. Continue until a pattern is found or all elements are processed.

#### Complexity
- **Time Complexity**: O(n), as each element is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), due to the stack used to store elements.

```csharp
public bool Find132patternOptimal(int[] nums) 
{
    int n = nums.Length;
    int third = Int32.MinValue;
    Stack<int> stack = new Stack<int>();

    // Traverse from the end of the list to the beginning
    for (int i = n - 1; i >= 0; i--) 
    {
        // If current element is less than the third element of 132 pattern
        if (nums[i] < third) 
        {
            return true;
        }
        
        // Pop elements from stack until we find something larger than nums[i]
        while (stack.Count > 0 && nums[i] > stack.Peek()) 
        {
            // Update third
            third = stack.Pop();
        }

        // Push current element onto the stack
        stack.Push(nums[i]);
    }
    return false;
}
```

This solution effectively utilizes a stack to achieve linear time complexity, providing an optimal solution to detect the "132" pattern in an array.

