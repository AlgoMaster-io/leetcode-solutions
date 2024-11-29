# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
```javascript
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)
function accountsMerge(accounts) {
    const parent = {};
    const owner = {};

    // Initialize parent to self and store owner for each email
    for (const account of accounts) {
        const ownerName = account[0];
        for (let i = 1; i < account.length; i++) {
            if (!parent.hasOwnProperty(account[i])) {
                parent[account[i]] = account[i]; // Each email is its own parent initially
                owner[account[i]] = ownerName; // Record owner of each email
            }
        }
    }

    // Union-Find: Union the first email with the rest in each account
    for (const account of accounts) {
        const firstEmail = find(account[1], parent);
        for (let i = 2; i < account.length; i++) {
            const nextEmail = find(account[i], parent);
            parent[nextEmail] = firstEmail;
        }
    }

    // Group emails by their root parent
    const unionedEmails = {};
    for (const email in parent) {
        const rootEmail = find(email, parent);
        if (!unionedEmails.hasOwnProperty(rootEmail)) {
            unionedEmails[rootEmail] = new Set();
        }
        unionedEmails[rootEmail].add(email);
    }

    // Construct result using owners and grouped emails
    const result = [];
    for (const rootEmail in unionedEmails) {
        const emails = Array.from(unionedEmails[rootEmail]);
        emails.sort();
        emails.unshift(owner[rootEmail]); // Add owner to the start
        result.push(emails);
    }

    return result;
}

// Helper function to find the root of an email
function find(email, parent) {
    if (parent[email] !== email) {
        parent[email] = find(parent[email], parent); // Path compression
    }
    return parent[email];
}
```

## Approach 2: DFS to Group Emails

### Solution
```javascript
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)
function accountsMerge(accounts) {
    const emailToName = {};
    const graph = {};

    // Build graph and map email to owner
    for (const account of accounts) {
        const name = account[0];
        for (let i = 1; i < account.length; i++) {
            emailToName[account[i]] = name;
            if (!graph.hasOwnProperty(account[i])) {
                graph[account[i]] = [];
            }
            if (i > 1) {
                graph[account[i - 1]].push(account[i]);
                graph[account[i]].push(account[i - 1]);
            }
        }
    }

    // Traverse graph to merge accounts
    const visited = new Set();
    const result = [];

    for (const email in emailToName) {
        if (!visited.has(email)) {
            const list = [];
            dfs(email, graph, visited, list);
            list.sort();
            list.unshift(emailToName[email]); // Add owner to the start
            result.push(list);
        }
    }

    return result;
}

// Helper function for DFS traversal
function dfs(email, graph, visited, list) {
    visited.add(email);
    list.push(email);
    for (const neighbor of graph[email]) {
        if (!visited.has(neighbor)) {
            dfs(neighbor, graph, visited, list);
        }
    }
}
```

