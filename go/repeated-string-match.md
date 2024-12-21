### [LeetCode Problem 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

### Table of Contents
- [Approach 1: Naive Method](#approach-1)
- [Approach 2: Optimized Method with String Concatenation](#approach-2)

---

### Approach 1: Naive Method

#### Intuition
The naive approach involves repeatedly appending string `A` to itself and checking if `B` becomes a substring of the repeated version of `A`. This will work because there's an upper bound on how many times you would need to repeat `A` for `B` to be a substring (at most `len(B) / len(A) + 2`).

#### Code
```go
func repeatedStringMatch(A string, B string) int {
    // Start with one copy of A
    repeatedA := A
    count := 1

    // Continue appending A until the length of repeatedA is >= length of B
    for len(repeatedA) < len(B) {
        repeatedA += A
        count++
    }

    // Check if B is now a substring of the repeated A
    if strings.Contains(repeatedA, B) {
        return count
    }

    // Append one more A and check again
    repeatedA += A
    if strings.Contains(repeatedA, B) {
        return count + 1
    }

    // If B is still not a substring, return -1
    return -1
}
```

#### Time Complexity
- **O(N * M)**: In the worst case, where `N` is the length of `A` and `M` is the length of `B`, we might need to append `A` up to a few hundred times and check for each combination, which leads to a nested loop.

#### Space Complexity
- **O(N * M)**: Space primarily used on the concatenated `repeatedA` string until the desired length.

---

### Approach 2: Optimized Method with String Concatenation

#### Intuition
Optimal solution recognizes that you only need to consider up to `len(B)/len(A) + 2` times of `A` appended. By focusing on the length and recognizing that `B` must fully exist within `A`, the solution can be derived efficiently by checking on increments of repeats and requires fewer direct substring checks.

#### Code
```go
func repeatedStringMatch(A string, B string) int {
    // Determine the minimum repetitions of A needed
    minRepeats := (len(B) - 1) / len(A) + 1

    // Generate the repeated A by repeating it minRepeats times
    repeatedA := strings.Repeat(A, minRepeats)

    // Check if B is a substring of the current repeatedA
    if strings.Contains(repeatedA, B) {
        return minRepeats
    }

    // Continuously check by appending one more A 
    repeatedA += A
    if strings.Contains(repeatedA, B) {
        return minRepeats + 1
    }

    // If B still not found, return -1
    return -1
}
```

#### Time Complexity
- **O(N + M)**: Time complexity reduced significantly due to efficient repetition and substring checking rather than recalculating every possible substring.

#### Space Complexity
- **O(N + M)**: Space remains in the similar bounds as multiple appends of A required until B is found as a substring.

---

Each approach incrementally increases efficiency and reduces unnecessary computational steps, leaning more towards smart uses of native string operations to solve the problem effectively.

