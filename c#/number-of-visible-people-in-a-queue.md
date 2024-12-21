# [Leetcode 1944: Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Stack-based Monotonic Decreasing Stack](#stack-based-monotonic-decreasing-stack)

### Brute Force Approach

#### Intuition:
The brute force approach involves checking each pair of people in the queue to determine visibility. A person is visible to another if they are taller than all the persons in between. We compare each person with every other person that comes after them in the queue.

#### Steps:
1. Initialize an array `result` of the same size as the input `heights` array to store the number of visible people for each person.
2. For each person in the queue (from left to right), iterate over each subsequent person in the queue.
3. Count the number of people who are shorter and directly visible to the current person.
4. Update the `result` array with the count.

```csharp
public int[] CanSeePersonsCount(int[] heights) {
    int n = heights.Length;
    int[] result = new int[n];

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = i + 1; j < n; j++) {
            if (heights[j] >= heights[i]) {
                count++;
                break; // Can't see past this person, so break
            } else {
                count++; // Count this person as visible
            }
        }
        result[i] = count;
    }

    return result;
}
```

#### Time Complexity:
- O(n^2) because for each person, you might need to compare with every other person after them.

#### Space Complexity:
- O(n) for the `result` array.

### Stack-based Monotonic Decreasing Stack

#### Intuition:
Instead of comparing each person with everyone after them, we can use a stack to efficiently determine the number of visible people. The idea is to maintain a stack of indices of the people such that their heights are in decreasing order. As we process each person, we can pop from the stack and update the result when we find someone taller.

#### Steps:
1. Initialize a `result` array and a stack to store indices.
2. Iterate over the `heights` array from right to left (since we're dealing with who can see to the right).
3. For each person, pop the stack until we find someone taller or until the stack is empty. Count the number of pops.
4. If the stack is not empty after the pops, increment the count by one because this person also sees someone taller.
5. Update the `result` array with the count.
6. Push the current index onto the stack.

```csharp
public int[] CanSeePersonsCount(int[] heights) {
    int n = heights.Length;
    int[] result = new int[n];
    Stack<int> stack = new Stack<int>();

    for (int i = n - 1; i >= 0; i--) {
        int count = 0;
        // While the stack is not empty and the current person is taller
        while (stack.Count > 0 && heights[i] > heights[stack.Peek()]) {
            stack.Pop();
            count++;
        }
        // If the stack is not empty, the person sees one more taller person
        if (stack.Count > 0) {
            count++;
        }
        result[i] = count;
        stack.Push(i);
    }

    return result;
}
```

#### Time Complexity:
- O(n) because each person is pushed and popped from the stack at most once.

#### Space Complexity:
- O(n) for the stack and result array.


These approaches lead us from a naive solution to a more optimized method utilizing a stack to maintain a record of visibility efficiently.

