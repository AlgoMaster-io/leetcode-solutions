## [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

### Approaches:
- [Approach 1: DFS and HashMap](#approach-1-dfs-and-hashmap)
- [Approach 2: Union Find](#approach-2-union-find)

### Approach 1: DFS and HashMap

**Intuition:**
The task is essentially about grouping connected components (emails) under the same owner. Using DFS, we can find all connected components (i.e., all emails linked to each other) and finally concatenate the results under the same account.

1. Use a map to associate each email with its owner and build a graph connecting these emails.
2. Apply DFS to traverse through the graph, marking each email visited and gathering all connected emails (representing one complete account).
3. For each account's first email, perform DFS to fetch all connected emails, sort them, and store under the corresponding owner.

**Code:**

```java
import java.util.*;

public class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap<>();
        Map<String, List<String>> graph = new HashMap<>();
        
        // Build graph
        for (List<String> account : accounts) {
            String name = account.get(0);
            for (int i = 1; i < account.size(); i++) {
                emailToName.put(account.get(i), name);
                graph.putIfAbsent(account.get(i), new ArrayList<>());
                // Connect the first email with the current email
                if (i == 1) continue;
                graph.get(account.get(1)).add(account.get(i));
                graph.get(account.get(i)).add(account.get(1));
            }
        }
        
        // Perform DFS
        Set<String> visited = new HashSet<>();
        List<List<String>> res = new ArrayList<>();
        for (String email : graph.keySet()) {
            if (!visited.contains(email)) {
                List<String> list = new ArrayList<>();
                dfs(email, graph, visited, list);
                Collections.sort(list); // Sort emails
                list.add(0, emailToName.get(email));
                res.add(list);
            }
        }
        
        return res;
    }
    
    private void dfs(String email, Map<String, List<String>> graph, Set<String> visited, List<String> list) {
        visited.add(email);
        list.add(email);
        for (String neighbor : graph.get(email)) {
            if (!visited.contains(neighbor)) {
                dfs(neighbor, graph, visited, list);
            }
        }
    }
}
```

**Time Complexity:** O(N \* K \* log(K)), where N is the number of accounts and K is the number of emails in an account. Sorting takes K * log(K).
**Space Complexity:** O(N \* K), storing graph, email-to-name map.

---

### Approach 2: Union Find

**Intuition:**
Union-Find (Disjoint Set Union) is a perfect fit for grouping connected components and is typically used in problems of this pattern. We can take advantage of it to union all emails under the same account and later find and collect emails by their root (or parent).

1. Initialize Union-Find structures and map each email to its parent.
2. Union emails and organize them, setting their first email as the "parent".
3. Use a Map to group emails by their parents/components.
4. Append sorted grouped emails, prefixed by their owner's name, to the result list.

**Code:**

```java
import java.util.*;

public class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap<>();
        Map<String, String> parent = new HashMap<>();
        
        // Initialize Union-Find structures
        for (List<String> account : accounts) {
            String name = account.get(0);
            for (int i = 1; i < account.size(); i++) {
                emailToName.put(account.get(i), name); 
                parent.put(account.get(i), account.get(i));
            }
        }
        
        // Union accounts
        for (List<String> account : accounts) {
            String p = find(account.get(1), parent);
            for (int i = 2; i < account.size(); i++) {
                parent.put(find(account.get(i), parent), p);
            }
        }
        
        // Group accounts by root email
        Map<String, List<String>> unions = new HashMap<>();
        for (String email : parent.keySet()) {
            String root = find(email, parent);
            unions.putIfAbsent(root, new ArrayList<>());
            unions.get(root).add(email);
        }
        
        // Collect result
        List<List<String>> res = new ArrayList<>();
        for (String root : unions.keySet()) {
            List<String> emails = unions.get(root);
            Collections.sort(emails); // Sort emails
            emails.add(0, emailToName.get(root)); // Add the owner
            res.add(emails);
        }
        
        return res;
    }
    
    private String find(String email, Map<String, String> parent) {
        if (!email.equals(parent.get(email))) {
            parent.put(email, find(parent.get(email), parent));
        }
        return parent.get(email);
    }
}
```

**Time Complexity:** O(N \* K * log(K)), similar to the DFS approach with additional log factor for find operation.
**Space Complexity:** O(N \* K), storing parent and email mappings.

