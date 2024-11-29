# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
```java
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)
import java.util.*;

public class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        // Map each email to a parent email
        Map<String, String> parent = new HashMap<>();
        // Map to retrieve the name associated with each email
        Map<String, String> owner = new HashMap<>();
        
        // Initialize parent to self and store owner for each email
        for (List<String> account : accounts) {
            String ownerName = account.get(0);
            for (int i = 1; i < account.size(); i++) {
                parent.put(account.get(i), account.get(i)); // Each email is its own parent initially
                owner.put(account.get(i), ownerName); // Record owner of each email
            }
        }
        
        // Union-Find: Union the first email with the rest in each account
        for (List<String> account : accounts) {
            String firstEmail = find(account.get(1), parent);
            for (int i = 2; i < account.size(); i++) {
                String nextEmail = find(account.get(i), parent);
                parent.put(nextEmail, firstEmail);
            }
        }
        
        // Group emails by their root parent
        Map<String, TreeSet<String>> unionedEmails = new HashMap<>();
        for (String email : parent.keySet()) {
            String rootEmail = find(email, parent);
            unionedEmails.computeIfAbsent(rootEmail, x -> new TreeSet<>()).add(email);
        }
        
        // Construct result using owners and grouped emails
        List<List<String>> result = new ArrayList<>();
        for (String rootEmail : unionedEmails.keySet()) {
            List<String> emails = new ArrayList<>(unionedEmails.get(rootEmail));
            emails.add(0, owner.get(rootEmail)); // Add owner to the start
            result.add(emails);
        }
        
        return result;
    }
    
    // Helper method to find the root of an email
    private String find(String email, Map<String, String> parent) {
        if (!parent.get(email).equals(email)) {
            parent.put(email, find(parent.get(email), parent)); // Path compression
        }
        return parent.get(email);
    }
}
```

## Approach 2: DFS to Group Emails

### Solution
```java
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)
import java.util.*;

public class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap<>();
        Map<String, List<String>> graph = new HashMap<>();
        
        // Build graph and map email to owner
        for (List<String> account : accounts) {
            String name = account.get(0);
            for (int i = 1; i < account.size(); i++) {
                emailToName.put(account.get(i), name);
                graph.computeIfAbsent(account.get(i), x -> new ArrayList<>());
                if (i == 1) continue;
                // Create undirected connections
                graph.get(account.get(i - 1)).add(account.get(i));
                graph.get(account.get(i)).add(account.get(i - 1));
            }
        }
        
        // Traverse graph to merge accounts
        Set<String> visited = new HashSet<>();
        List<List<String>> result = new ArrayList<>();
        
        for (String email : emailToName.keySet()) {
            if (!visited.contains(email)) {
                List<String> list = new ArrayList<>();
                dfs(email, graph, visited, list);
                Collections.sort(list);
                list.add(0, emailToName.get(email)); // Add owner to the start
                result.add(list);
            }
        }
        
        return result;
    }
    
    // Helper method for DFS traversal
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

