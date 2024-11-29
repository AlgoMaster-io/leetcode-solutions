# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
go
```go
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)
package main

import (
	"sort"
)

func accountsMerge(accounts [][]string) [][]string {
	parent := make(map[string]string)
	owner := make(map[string]string)

	// Initialize parent to self and store owner for each email
	for _, account := range accounts {
		ownerName := account[0]
		for i := 1; i < len(account); i++ {
			parent[account[i]] = account[i]
			owner[account[i]] = ownerName
		}
	}

	// Union-Find: Union the first email with the rest in each account
	for _, account := range accounts {
		firstEmail := find(account[1], parent)
		for i := 2; i < len(account); i++ {
			nextEmail := find(account[i], parent)
			parent[nextEmail] = firstEmail
		}
	}

	// Group emails by their root parent
	unionedEmails := make(map[string][]string)
	for email := range parent {
		rootEmail := find(email, parent)
		unionedEmails[rootEmail] = append(unionedEmails[rootEmail], email)
	}

	// Construct result using owners and grouped emails
	var result [][]string
	for rootEmail, emails := range unionedEmails {
		sort.Strings(emails)
		emails = append([]string{owner[rootEmail]}, emails...)
		result = append(result, emails)
	}

	return result
}

// Helper method to find the root of an email
func find(email string, parent map[string]string) string {
	if parent[email] != email {
		parent[email] = find(parent[email], parent) // Path compression
	}
	return parent[email]
}
```

## Approach 2: DFS to Group Emails

### Solution
go
```go
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)
package main

import (
	"sort"
)

func accountsMerge(accounts [][]string) [][]string {
	emailToName := make(map[string]string)
	graph := make(map[string][]string)

	// Build graph and map email to owner
	for _, account := range accounts {
		name := account[0]
		for i := 1; i < len(account); i++ {
			emailToName[account[i]] = name
			graph[account[i]] = graph[account[i]]
			if i == 1 {
				continue
			}
			// Create undirected connections
			graph[account[i-1]] = append(graph[account[i-1]], account[i])
			graph[account[i]] = append(graph[account[i]], account[i-1])
		}
	}

	// Traverse graph to merge accounts
	visited := make(map[string]bool)
	var result [][]string

	for email := range emailToName {
		if !visited[email] {
			var list []string
			dfs(email, graph, visited, &list)
			sort.Strings(list)
			list = append([]string{emailToName[email]}, list...)
			result = append(result, list)
		}
	}

	return result
}

// Helper method for DFS traversal
func dfs(email string, graph map[string][]string, visited map[string]bool, list *[]string) {
	visited[email] = true
	*list = append(*list, email)
	for _, neighbor := range graph[email] {
		if !visited[neighbor] {
			dfs(neighbor, graph, visited, list)
		}
	}
}
```

