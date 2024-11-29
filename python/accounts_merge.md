# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution

```python
# Time Complexity: O(A * logA), where A is the total number of emails
# Space Complexity: O(A)
from collections import defaultdict, deque

class Solution:
    def accountsMerge(self, accounts):
        parent = {}
        owner = {}

        # Initialize parent to self and store owner for each email
        for account in accounts:
            ownerName = account[0]
            for email in account[1:]:
                parent[email] = email  # Each email is its own parent initially
                owner[email] = ownerName  # Record owner of each email
        
        # Helper method to find the root of an email
        def find(email):
            if parent[email] != email:
                parent[email] = find(parent[email])  # Path compression
            return parent[email]
        
        # Union-Find: Union the first email with the rest in each account
        for account in accounts:
            firstEmail = find(account[1])
            for email in account[2:]:
                parent[find(email)] = firstEmail
        
        # Group emails by their root parent
        unionedEmails = defaultdict(set)
        for email in parent:
            rootEmail = find(email)
            unionedEmails[rootEmail].add(email)
        
        # Construct result using owners and grouped emails
        result = []
        for rootEmail, emails in unionedEmails.items():
            result.append([owner[rootEmail]] + sorted(emails))
        
        return result
```

## Approach 2: DFS to Group Emails

### Solution

```python
# Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
# Space Complexity: O(A)
from collections import defaultdict

class Solution:
    def accountsMerge(self, accounts):
        emailToName = {}
        graph = defaultdict(list)

        # Build graph and map email to owner
        for account in accounts:
            name = account[0]
            for email in account[1:]:
                emailToName[email] = name
                graph[email].append(account[1])
                graph[account[1]].append(email)

        # Traverse graph to merge accounts
        visited = set()
        result = []

        def dfs(email, list_):
            visited.add(email)
            list_.append(email)
            for neighbor in graph[email]:
                if neighbor not in visited:
                    dfs(neighbor, list_)

        for email in emailToName:
            if email not in visited:
                list_ = []
                dfs(email, list_)
                result.append([emailToName[email]] + sorted(list_))

        return result
```


