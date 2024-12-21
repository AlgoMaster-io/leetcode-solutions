# [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approaches
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Disjoint Set Union (Union-Find)](#approach-2-disjoint-set-union-union-find)

---

## Approach 1: Depth First Search (DFS)

### Intuition
The problem requires us to find connected components in terms of the email connections within the accounts. A natural way to explore connected components in graph problems is using Depth First Search (DFS). Here, each account can be thought of as a node, and edges exist between accounts that share an email. By constructing and traversing this graph, we can identify distinct user components.

### Steps
1. Create a graph where each email points to a list of emails (adjacency list).
2. Use a mapping from email to the corresponding account name.
3. Traverse each email using DFS to visit all connected emails (connected component) and collect them.
4. Collect and sort the emails for each connected component and append the result with respect to its owner's name.

### Code
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public IList<IList<string>> AccountsMerge(IList<IList<string>> accounts) {
        // Map each email to a list of emails representing the graph edges
        Dictionary<string, List<string>> emailGraph = new Dictionary<string, List<string>>();
        // Map email to account name
        Dictionary<string, string> emailToName = new Dictionary<string, string>();

        foreach (var account in accounts) {
            string name = account[0];
            for (int i = 1; i < account.Count; i++) {
                string email = account[i];
                emailToName[email] = name; // Map email to the name
                if (i == 1) continue;
                
                // Create bidirectional edge between current and previous emails
                string prevEmail = account[i - 1];
                if (!emailGraph.ContainsKey(email)) emailGraph[email] = new List<string>();
                if (!emailGraph.ContainsKey(prevEmail)) emailGraph[prevEmail] = new List<string>();
                emailGraph[email].Add(prevEmail);
                emailGraph[prevEmail].Add(email);
            }
        }

        // To track visited emails
        HashSet<string> visited = new HashSet<string>();
        // Result to store the final merged accounts
        IList<IList<string>> result = new List<IList<string>>();

        // Perform DFS for each unvisited email
        foreach (var email in emailGraph.Keys) {
            if (!visited.Contains(email)) {
                List<string> component = new List<string>();
                DFS(email, visited, emailGraph, component);
                component.Sort(); // Sorting for result format requirement
                component.Insert(0, emailToName[email]); // Add owner's name to the front
                result.Add(component);
            }
        }

        return result;
    }

    private void DFS(string email, HashSet<string> visited, Dictionary<string, List<string>> emailGraph, List<string> component) {
        visited.Add(email);
        component.Add(email);
        foreach (string neighbor in emailGraph[email]) {
            if (!visited.Contains(neighbor)) {
                DFS(neighbor, visited, emailGraph, component);
            }
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N * K log K), where N is the number of accounts and K is the average number of emails per account. Traversing the graph takes O(N * K) time, and sorting each component takes O(K log K).
- **Space Complexity**: O(N * K), as we store the graph and visited components.

---

## Approach 2: Disjoint Set Union (Union-Find)

### Intuition
The Union-Find algorithm provides an efficient way to manage a collection of disjoint sets and perform unions and finds efficiently. By treating each email as a node, we can union nodes that belong to the same account. Once we have processed unions for all accounts, connected components represent the merged accounts.

### Steps
1. Initialize a Union-Find data structure to maintain email connections.
2. Map each email to the corresponding account name.
3. Perform union operations for all emails belonging to the same account.
4. For each email in the account list, find its root and collect component emails.
5. Sort and output these components accordingly.

### Code
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public IList<IList<string>> AccountsMerge(IList<IList<string>> accounts) {
        // Union-Find helper class
        class UnionFind {
            private Dictionary<string, string> parent;

            public UnionFind() {
                parent = new Dictionary<string, string>();
            }

            public string Find(string x) {
                if (!parent.ContainsKey(x)) {
                    parent[x] = x;
                }
                if (x != parent[x]) {
                    parent[x] = Find(parent[x]); // Path compression
                }
                return parent[x];
            }

            public void Union(string x, string y) {
                string rootX = Find(x);
                string rootY = Find(y);
                if (rootX != rootY) {
                    parent[rootY] = rootX; // Union
                }
            }
        }

        UnionFind uf = new UnionFind();
        Dictionary<string, string> emailToName = new Dictionary<string, string>();

        // Union emails within the same account
        foreach (var account in accounts) {
            string name = account[0];
            for (int i = 1; i < account.Count; i++) {
                emailToName[account[i]] = name;
                if (i == 1) continue;
                uf.Union(account[i], account[i - 1]);
            }
        }

        // Group emails by their root parent
        Dictionary<string, List<string>> rootToEmails = new Dictionary<string, List<string>>();
        foreach (string email in emailToName.Keys) {
            string root = uf.Find(email);
            if (!rootToEmails.ContainsKey(root)) {
                rootToEmails[root] = new List<string>();
            }
            rootToEmails[root].Add(email);
        }

        // Create result based on grouped emails
        IList<IList<string>> result = new List<IList<string>>();
        foreach (var group in rootToEmails.Values) {
            group.Sort(); // Sort the emails
            string name = emailToName[group[0]];
            group.Insert(0, name);
            result.Add(group);
        }

        return result;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N * K log K), akin to the DFS approach; path compression and union operations are nearly constant amortized time.
- **Space Complexity**: O(N * K) for maintaining the Union-Find structure and email-to-name mappings.

