# [LeetCode 753: Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: De Bruijn Sequence (Optimal)](#approach-2-de-bruijn-sequence-optimal)

## Approach 1: Brute Force

### Intuition
The basic idea here is to use a brute force method to generate all possible combinations of the keypad locks and then check which combination is valid. This approach, while straightforward, might be computationally expensive due to generating and checking all possibilities.

### Detailed Steps
1. Generate all permutations of the numbers for the keypad.
2. Check each permutation to see if it covers all possible k-length sequences.
3. If a permutation satisfies the condition, it could be considered as a valid combination.

### Code
```go
func crackSafe(n int, k int) string {
    visited := make(map[string]bool)
    result := ""
    
    // Start with the initial sequence of n zeroes
    initialSeq := make([]rune, n)
    for i := 0; i < n; i++ {
        initialSeq[i] = '0'
    }
    
    dfs(string(initialSeq), visited, k, &result)
    result += string(initialSeq)
    
    return result
}

func dfs(currNode string, visited map[string]bool, k int, result *string) {
    for i := 0; i < k; i++ {
        newSequence := currNode + string('0'+i)
        
        if !visited[newSequence] {
            visited[newSequence] = true

            // Recurse further with the new sequence by dropping the first character
            dfs(newSequence[1:], visited, k, result)
            *result += string('0' + i)
        }
    }
}
```

### Time Complexity
- Time complexity: \(O(k^n)\), as it tries all possible strings of length n using k symbols.

### Space Complexity
- Space complexity: \(O(k^n)\), for storing visited strings.

## Approach 2: De Bruijn Sequence (Optimal)

### Intuition
The De Bruijn sequence is a cyclic sequence that contains every possible k-length sequence exactly once. This provides an elegant solution using the properties of De Bruijn sequences that can generate the desired solution in an optimal manner.

### Detailed Steps
1. Treat the sequence generation as a modified depth-first search over a graph of sequences.
2. Each node in this graph represents a sequence, and each edge represents an additional digit being added.
3. Use a Eulerian path approach to visit each sequence exactly once.

### Code
```go
func crackSafe(n int, k int) string {
    visted := make(map[string]bool)
    result := ""

    initialStr := make([]rune, n-1)
    for i := range initialStr {
        initialStr[i] = '0'
    }
    
    dfs(string(initialStr), k, &result, visted)
    result += string(initialStr)
    
    return result
}

func dfs(node string, k int, result *string, visted map[string]bool) {
    for i := 0; i < k; i++ {
        edge := node + string('0'+i)
        
        if !visted[edge] {
            visted[edge] = true
            dfs(edge[1:], k, result, visted)
            *result += string('0' + i)
        }
    }
}
```

### Time Complexity
- Time complexity: \(O(k^n)\), as theoretically, it visits each sequence once.

### Space Complexity
- Space complexity: \(O(k^n)\), for keeping track of visited nodes in this graph.

By utilizing De Bruijn sequence properties, we ensure an optimal approach to cover all combinations with minimal redundancy. The code uses depth-first traversal to construct the sequence efficiently.

