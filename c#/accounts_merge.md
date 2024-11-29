# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
csharp
```csharp
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> AccountsMerge(IList<IList<string>> accounts) {
        // Map each email to a parent email
        Dictionary<string, string> parent = new Dictionary<string, string>();
        // Map to retrieve the name associated with each email
        Dictionary<string, string> owner = new Dictionary<string, string>();
        
        // Initialize parent to self and store owner for each email
        foreach (var account in accounts) {
            string ownerName = account[0];
            for (int i = 1; i < account.Count; i++) {
                if (!parent.ContainsKey(account[i])) {
                    parent[account[i]] = account[i]; // Each email is its own parent initially
                    owner[account[i]] = ownerName; // Record owner of each email
                }
            }
        }
        
        // Union-Find: Union the first email with the rest in each account
        foreach (var account in accounts) {
            string firstEmail = Find(account[1], parent);
            for (int i = 2; i < account.Count; i++) {
                string nextEmail = Find(account[i], parent);
                parent[nextEmail] = firstEmail;
            }
        }
        
        // Group emails by their root parent
        Dictionary<string, SortedSet<string>> unionedEmails = new Dictionary<string, SortedSet<string>>();
        foreach (var email in parent.Keys) {
            string rootEmail = Find(email, parent);
            if (!unionedEmails.ContainsKey(rootEmail))
                unionedEmails[rootEmail] = new SortedSet<string>();
            unionedEmails[rootEmail].Add(email);
        }
        
        // Construct result using owners and grouped emails
        List<IList<string>> result = new List<IList<string>>();
        foreach (var rootEmail in unionedEmails.Keys) {
            List<string> emails = new List<string>(unionedEmails[rootEmail]);
            emails.Insert(0, owner[rootEmail]); // Add owner to the start
            result.Add(emails);
        }
        
        return result;
    }
    
    // Helper method to find the root of an email
    private string Find(string email, Dictionary<string, string> parent) {
        if (parent[email] != email) {
            parent[email] = Find(parent[email], parent); // Path compression
        }
        return parent[email];
    }
}
```

## Approach 2: DFS to Group Emails

### Solution
csharp
```csharp
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> AccountsMerge(IList<IList<string>> accounts) {
        Dictionary<string, string> emailToName = new Dictionary<string, string>();
        Dictionary<string, List<string>> graph = new Dictionary<string, List<string>>();
        
        // Build graph and map email to owner
        foreach (var account in accounts) {
            string name = account[0];
            for (int i = 1; i < account.Count; i++) {
                emailToName[account[i]] = name;
                if (!graph.ContainsKey(account[i]))
                    graph[account[i]] = new List<string>();
                if (i == 1) continue;
                // Create undirected connections
                graph[account[i - 1]].Add(account[i]);
                graph[account[i]].Add(account[i - 1]);
            }
        }
        
        // Traverse graph to merge accounts
        HashSet<string> visited = new HashSet<string>();
        List<IList<string>> result = new List<IList<string>>();
        
        foreach (var email in emailToName.Keys) {
            if (!visited.Contains(email)) {
                List<string> list = new List<string>();
                DFS(email, graph, visited, list);
                list.Sort();
                list.Insert(0, emailToName[email]); // Add owner to the start
                result.Add(list);
            }
        }
        
        return result;
    }
    
    // Helper method for DFS traversal
    private void DFS(string email, Dictionary<string, List<string>> graph, HashSet<string> visited, List<string> list) {
        visited.Add(email);
        list.Add(email);
        foreach (var neighbor in graph[email]) {
            if (!visited.Contains(neighbor)) {
                DFS(neighbor, graph, visited, list);
            }
        }
    }
}
```

