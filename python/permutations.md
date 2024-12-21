# [Leetcode 46: Permutations](https://leetcode.com/problems/permutations/)

## Approaches

1. [Backtracking](#backtracking)
2. [Iterative / BFS Approach](#iterative--bfs-approach)

---

### Backtracking

Backtracking is a common algorithmic technique for solving problems incrementally by trying partial solutions and then abandoning them if they are not valid. For permutations, the idea is to build permutations by exploring each possible element choice at every position recursively.

#### Intuition

1. The basic idea is to fix a number in the first position and then recursively permute the rest of the numbers.
2. We start with an empty permutation and at each step, add a number from the `nums` list to the current permutation.
3. If the current permutation length equals the `nums` length, we've found a permutation.
4. We use a backtracking technique to arrange all possible numbers in every position, making sure not to repeat numbers in a permutation.

```python
def permute(nums):
    result = []
    
    # Helper function for backtracking
    def backtrack(path, options):
        # If the path includes all numbers, add the current permutation to the result
        if not options:
            result.append(path)
            return
        
        # Iterate through the available options
        for i in range(len(options)):
            # Take the current number and exclude it from the remaining options
            backtrack(path + [options[i]], options[:i] + options[i+1:])
    
    backtrack([], nums)
    return result

# Test the function with example input
print(permute([1, 2, 3]))
```

**Time Complexity**: O(N * N!), where N is the number of elements in the `nums` list. This is because there are N! permutations and each permutation takes O(N) time to be added to the result list.

**Space Complexity**: O(N!) due to the space used to hold all permutations in the result list. The call stack for recursion could also go up to O(N).

---

### Iterative / BFS Approach

The iterative or BFS approach constructs permutations by gradually building from an initial state of empty permutations and progressively adding numbers to those partial permutations.

#### Intuition

1. Start with a list containing an empty permutation: `[[]]`.
2. For each number in `nums`, for each existing permutation in the list of permutations, create new permutations by adding the current number at every possible position.
3. Collect these new permutations and replace the existing list of permutations with these new permutations.
4. Continue until all numbers are inserted into each permutation.

```python
def permute_iterative(nums):
    permutations = [[]]
    
    for num in nums:
        new_permutations = []
        for perm in permutations:
            # Insert the current number into every position of the existing permutation
            for i in range(len(perm) + 1):
                new_permutations.append(perm[:i] + [num] + perm[i:])
        
        permutations = new_permutations
    
    return permutations

# Test the function with example input
print(permute_iterative([1, 2, 3]))
```

**Time Complexity**: O(N * N!), This approach requires inserting each number into N! different permutations, each insertion takes O(N) time due to the need to create a new list.

**Space Complexity**: O(N!) as we need to store all permutations and the new combinations during the iteration.

--- 

These solutions expose two efficient ways of generating all permutations of a list, using recursive backtracking and iterative construction. Each approach has its own use-case depending on whether conceptual simplicity and elegance or iterative construction and non-recursive methods are preferred.

