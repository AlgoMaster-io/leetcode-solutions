# [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Table of Contents

1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Union-Find (Disjoint Set Union, DSU)](#approach-2-union-find-disjoint-set-union-dsu)

---

## Approach 1: Brute Force

### Intuition

The brute force approach involves iterating over all account pairs and merging them whenever they share a common email. Repeated checking and merging process continues until no more merges are possible. This approach can become quite inefficient as the number of accounts grows because merging requires checking every account against all others that haven't been merged yet.

### Steps

1. Iterate over each account and create a map where the key is the email and the value is a list of account indices that contain the email.
2. Perform a double iteration over all accounts. For each unique account pair, check if they have a common email by looking into the map.
3. If a common email is found, merge the accounts, keeping track of merged indices.
4. Continue this process iteratively until no further merges can be done.
5. Collect unique merged accounts and sort the emails lexicographically.

### Code

```go
// Brute Force Solution
func accountsMergeBruteForce(accounts [][]string) [][]string {
    // Create a map to track email to account mappings
    emailToIndex := make(map[string][]int)
    for i, account := range accounts {
        for _, email := range account[1:] {
            emailToIndex[email] = append(emailToIndex[email], i)
        }
    }
    
    // Merge accounts based on common emails
    visited := make([]bool, len(accounts))
    result := [][]string{}
    stack := []int{}
    
    for i, account := range accounts {
        if visited[i] {
            continue
        }
        emails := make(map[string]bool)
        stack = append(stack, i)
        for len(stack) > 0 {
            curr := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            if visited[curr] {
                continue
            }
            visited[curr] = true
            for _, email := range accounts[curr][1:] {
                emails[email] = true
                for _, neighbor := range emailToIndex[email] {
                    if !visited[neighbor] {
                        stack = append(stack, neighbor)
                    }
                }
            }
        }
        merged := []string{account[0]}
        for email := range emails {
            merged = append(merged, email)
        }
        sort.Strings(merged[1:]) // sort emails lexicographically, exclude the first element (name)
        result = append(result, merged)
    }
    return result
}
```

### Complexity Analysis

- **Time Complexity**: O((N^2) * M log M), where N is the number of accounts and M is the maximum number of emails in an account. This is because for each account, we check with every other account, and sorting emails is an O(M log M) operation.
- **Space Complexity**: O(N * M), where we store all emails and their associated accounts.

---

## Approach 2: Union-Find (Disjoint Set Union, DSU)

### Intuition

Given the inefficiencies of the brute force approach, a more efficient solution involves using a union-find data structure. The idea is to treat each email as a node in a graph and each account as a set of connected nodes (i.e., through the shared emails). Union-find helps in grouping these nodes (emails) efficiently, allowing us to identify connected components.

### Steps

1. Initialize a parent map to manage the union-find structure and rank array for optimization.
2. For each account, link all emails together. Choose one email as a representative (parent) for the account.
3. Use union and find operations to merge emails into the same connected component if they belong to the same account.
4. Group emails by their roots, which represent distinct connected components. Each group of emails corresponds to a unique merged account.
5. Construct the result by combining the owner's name with their emails and sorting the email list lexicographically.

### Code

```go
// Union-Find Solution
func accountsMergeUnionFind(accounts [][]string) [][]string {
    parent := make(map[string]string)
    owner := make(map[string]string)
    emailToEmail := make(map[string]string)
    
    // Initialize union-find structure for each email
    for _, account := range accounts {
        name := account[0]
        for _, email := range account[1:] {
            parent[email] = email // self parent
            owner[email] = name
        }
    }
    
    // Union operation: link all emails in a single account
    for _, account := range accounts {
        firstEmail := account[1]
        for _, email := range account[2:] {
            union(parent, firstEmail, email)
        }
    }
    
    // Group emails by their root parent
    for _, account := range accounts {
        for _, email := range account[1:] {
            rootEmail := find(parent, email)
            if emailToEmail[rootEmail] == "" {
                emailToEmail[rootEmail] = email
            } else {
                emailToEmail[rootEmail] += fmt.Sprintf(",%s", email)
            }
        }
    }
    
    // Construct the merged accounts using the owner name and sorted email groups
    result := [][]string{}
    for rootEmail, emails := range emailToEmail {
        splitEmails := strings.Split(emails, ",")
        sort.Strings(splitEmails)
        mergedAccount := append([]string{owner[rootEmail]}, splitEmails...)
        result = append(result, mergedAccount)
    }
    return result
}

// Find with path compression
func find(parent map[string]string, s string) string {
    if parent[s] != s {
        parent[s] = find(parent, parent[s])
    }
    return parent[s]
}

// Union two nodes
func union(parent map[string]string, s1, s2 string) {
    root1 := find(parent, s1)
    root2 := find(parent, s2)
    if root1 != root2 {
        parent[root1] = root2
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(N * M * α(NM)), where N is the number of accounts, M is the maximum number of emails per account, and α is the inverse Ackermann function, which is very close to a constant. This accounts for efficiently finding and union operations.
- **Space Complexity**: O(N * M), primarily for the union-find structure storing email-parent links and additional storage for owner information and grouped emails.

---

By using the union-find approach, we achieve a much more scalable solution, allowing for efficiently merging large numbers of accounts with potentially overlapping emails.

