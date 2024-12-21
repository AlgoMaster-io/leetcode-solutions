# [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approaches
1. [Naive Merge with Brute Force Checking](#1-naive-merge-with-brute-force-checking)
2. [Union-Find (Disjoint Set Union) Approach](#2-union-find-disjoint-set-union-approach)

---

## 1. Naive Merge with Brute Force Checking

### Intuition
In this naive approach, we attempt to merge accounts by iterating through each account and merging accounts that share at least one common email address. This approach leverages the fact that if two accounts share an email, they belong to the same user.

### Approach
- Iterate over each account and maintain a merged list.
- For each account, check against the current list of merged accounts.
- If an account shares an email with a merged account, merge them by merging their email sets.
- If no shared email is found, add it as a new entry in the merged list.
- Use a set to manage emails within accounts for quick lookup of common emails.

```python
def accountsMerge(accounts):
    from collections import defaultdict

    merged_accounts = []

    # Function to merge two accounts based on common emails
    def merge(account1, account2):
        if account1[0] != account2[0]:
            return False
        
        emails1 = set(account1[1:])
        emails2 = set(account2[1:])
        
        # Check if there's any common email
        if emails1 & emails2:
            merged_emails = sorted(emails1 | emails2)
            merged_accounts.remove(account1)
            merged_accounts.remove(account2)
            merged_accounts.append([account1[0]] + merged_emails)
            return True
        return False
    
    for account in accounts:
        found = False
        for m_account in list(merged_accounts):
            if merge(m_account, account):
                found = True
                break
        
        if not found:
            merged_accounts.append(account)
    
    return merged_accounts

# Time Complexity: O(N^2 * MlogM), where N is the number of accounts and M is the average number of emails per account.
# Space Complexity: O(N * M), for storing the emails in merged_accounts.
```

## 2. Union-Find (Disjoint Set Union) Approach

### Intuition
By modeling the problem using a Union-Find (Disjoint Set Union) data structure, we effectively group together emails belonging to the same user by finding connected components. This method is more efficient and elegant as it allows us to repeatedly unify emails that are directly or indirectly connected.

### Approach
- Use a Union-Find data structure to link emails associated with the same account.
- Map each email to an ID and apply union operations for emails within the same account.
- After processing all accounts, find connected components by checking root parents for each email.
- Group all emails having the same root and associate them back to the user name to form the final merged account list.

```python
def accountsMerge(accounts):
    from collections import defaultdict

    parent = {}

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    # Step 1: Union emails belonging to the same person
    for account in accounts:
        first_email = account[1]
        for email in account[1:]:
            if email not in parent:
                parent[email] = email
            if first_email not in parent:
                parent[first_email] = first_email
            parent[find(email)] = find(first_email)

    # Step 2: Aggregate emails by root parent
    email_to_name = {}
    components = defaultdict(list)

    for account in accounts:
        name = account[0]
        for email in account[1:]:
            root_email = find(email)
            components[root_email].append(email)
            email_to_name[email] = name

    # Step 3: Construct result
    merged_accounts = []
    for emails in components.values():
        name = email_to_name[emails[0]]
        merged_accounts.append([name] + sorted(list(set(emails))))

    return merged_accounts

# Time Complexity: O(N * M * α(NM)), where N is the number of accounts, M is the number of emails per account, and α is the inverse Ackermann function.
# Space Complexity: O(N * M), for storing the parent pointers and email map.
```

This second approach is significantly more efficient, especially for large datasets, as it minimizes redundant checking and merges through the powerful union-find paradigm.

