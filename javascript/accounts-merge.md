# [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Solutions:
- [Approach 1: Graph using DFS](#approach-1-graph-using-dfs)
- [Approach 2: Union-Find](#approach-2-union-find)

### Approach 1: Graph using DFS

**Intuition:**

The problem can be visualized as finding connected components in a graph, where each email address is a node, and an edge exists between two emails if they are part of the same account. The accounts can be merged by identifying these connected components, which can be done efficiently using Depth First Search (DFS).

**Steps:**
1. Construct a graph where each email is a node and there is an undirected edge between any two emails belonging to the same account.
2. Use DFS to traverse each connected component, gathering all emails in that component.
3. For each connected component (set of emails), retrieve the corresponding account name and sort the emails.
4. Collect the results in the required format.

**Code:**

```javascript
function accountsMerge(accounts) {
    const emailGraph = {};  // Graph relation
    const emailToName = {};  // Map an email to a name

    // Step 1: Build the graph
    for (let [name, ...emails] of accounts) {
        for (let email of emails) {
            if (!emailGraph[email]) emailGraph[email] = [];
            emailGraph[emails[0]].push(email);
            emailGraph[email].push(emails[0]);
            emailToName[email] = name;
        }
    }

    const visited = new Set();
    const result = [];

    // Step 2: DFS to explore connected components
    for (let email in emailGraph) {
        if (!visited.has(email)) {
            const stack = [email];
            const component = [];

            while (stack.length > 0) {
                const currentEmail = stack.pop();
                if (!visited.has(currentEmail)) {
                    visited.add(currentEmail);
                    component.push(currentEmail);
                    for (let neighbor of emailGraph[currentEmail]) {
                        stack.push(neighbor);
                    }
                }
            }

            // Sort and format the found component
            component.sort();
            result.push([emailToName[email], ...component]);
        }
    }

    return result;
}
```

**Time Complexity:** `O(N * K log K)`, where `N` is the number of accounts and `K` is the maximum number of emails in an account (due to sorting of emails).

**Space Complexity:** `O(N * K)` for the graph representation and visited set.

### Approach 2: Union-Find

**Intuition:**

The Union-Find method or Disjoint Set Union (DSU) is an efficient way to solve the problem of finding connected components. Each email is treated as a node, and accounts are merged by uniting disjoint email nodes.

**Steps:**
1. Initialize the Union-Find data structure to manage connections between emails.
2. Traverse the accounts to unify the emails belonging to the same account.
3. Use the union-find structure to find the root or parent of each email, which helps to identify connected components.
4. Collect emails connected to the same root, sort them, and retrieve the owners name.

**Code:**

```javascript
class UnionFind {
    constructor() {
        this.parent = {};
    }

    find(email) {
        if (this.parent[email] !== email) {
            this.parent[email] = this.find(this.parent[email]);
        }
        return this.parent[email];
    }

    union(email1, email2) {
        const root1 = this.find(email1);
        const root2 = this.find(email2);
        if (root1 !== root2) {
            this.parent[root2] = root1;
        }
    }

    add(email) {
        if (!this.parent[email]) {
            this.parent[email] = email;
        }
    }
}

function accountsMerge(accounts) {
    const uf = new UnionFind();
    const emailToName = {};
    
    // Step 1: Build the Union-Find structure
    for (let [name, ...emails] of accounts) {
        for (let email of emails) {
            uf.add(email);
            uf.union(emails[0], email);
            emailToName[email] = name;
        }
    }

    // Step 2: Find connected components
    const components = {};
    for (let email of Object.keys(emailToName)) {
        const root = uf.find(email);
        if (!components[root]) components[root] = [];
        components[root].push(email);
    }

    const result = [];
    for (let root in components) {
        const emails = components[root];
        emails.sort();
        result.push([emailToName[root], ...emails]);
    }

    return result;
}
```

**Time Complexity:** `O(N * K * α(N))` where `α` is the Inverse Ackermann function, which is very slow-growing, simplifying to amortized `O(K)` time per operation in practice.

**Space Complexity:** `O(N * K)` for storing parent pointers and mapping emails to names.

