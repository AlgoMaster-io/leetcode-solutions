# [Leetcode 721: Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approaches

- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Union-Find](#approach-2-union-find)

---

## Approach 1: Depth First Search (DFS)

### Intuition
The problem can be visualized as a graph problem where each account's email list is a node and shared emails form edges between nodes. We can perform a DFS to traverse through each group of connected nodes and merge corresponding accounts. 

### Steps:
1. Build an adjacency list where each email points to a list of connected emails.
2. Use a set to keep track of visited emails.
3. For each unvisited email, perform a DFS to collect all connected emails.
4. Sort the collected emails and construct a merged account.
5. Return the list of merged accounts.

### Code
```typescript
function accountsMerge(accounts: string[][]): string[][] {
    const emailGraph: {[email: string]: string[]} = {};
    const emailToName: {[email: string]: string} = {};
    
    // Create graph and email + name mapping
    for (const account of accounts) {
        const name = account[0];
        for (let i = 1; i < account.length; i++) {
            emailToName[account[i]] = name;
            if (!emailGraph[account[i]]) emailGraph[account[i]] = [];
            if (i === 1) continue;
            emailGraph[account[i]].push(account[i - 1]);
            emailGraph[account[i - 1]].push(account[i]);
        }
    }

    const visited: Set<string> = new Set();
    const dfs = (email: string, component: string[]) => {
        visited.add(email);
        component.push(email);
        for (const neighbor of emailGraph[email] || []) {
            if (!visited.has(neighbor)) {
                dfs(neighbor, component);
            }
        }
    };

    const mergedAccounts: string[][] = [];

    // Traverse and merge accounts
    for (const email in emailGraph) {
        if (!visited.has(email)) {
            const component: string[] = [];
            dfs(email, component);
            component.sort();
            mergedAccounts.push([emailToName[email], ...component]);
        }
    }

    return mergedAccounts;
}
```

### Time Complexity
- Construction of graph: O(N * K) where N is the number of accounts, and K is the number of emails per account.
- DFS traversal: O(N * K).
- Sorting the connected component: O(K log K).
Total: O(N * K log K).

### Space Complexity
- Adjacency list and name mapping: O(N * K).

---

## Approach 2: Union-Find

### Intuition
We can use the Union-Find data structure to group emails. The idea is to treat each email as a node and merge emails belonging to the same account by indexing them into the Union-Find data structure.

### Steps:
1. Initialize Union-Find structure where each email points to itself initially.
2. Process each account, and unify all emails belonging to it.
3. Iterate over all emails and find their root representative, effectively grouping connected components.
4. Collect the components using their root and arrange them under their respective account.

### Code
```typescript
class UnionFind {
    parent: Map<string, string>;

    constructor() {
        this.parent = new Map();
    }

    find(x: string) {
        if (this.parent.get(x) !== x) {
            this.parent.set(x, this.find(this.parent.get(x)!));
        }
        return this.parent.get(x)!;
    }

    union(x: string, y: string) {
        const rootX = this.find(x);
        const rootY = this.find(y);
        if (rootX !== rootY) {
            this.parent.set(rootX, rootY);
        }
    }

    add(x: string) {
        if (!this.parent.has(x)) {
            this.parent.set(x, x);
        }
    }
}

function accountsMerge(accounts: string[][]): string[][] {
    const uf = new UnionFind();
    const emailToName: Map<string, string> = new Map();
    
    for (const account of accounts) {
        const name = account[0];
        for (let i = 1; i < account.length; i++) {
            uf.add(account[i]);
            emailToName.set(account[i], name);
            if (i === 1) continue;
            uf.union(account[i], account[i - 1]);
        }
    }
    
    const components: Map<string, string[]> = new Map();
    for (const email of emailToName.keys()) {
        const root = uf.find(email);
        if (!components.has(root)) {
            components.set(root, []);
        }
        components.get(root)!.push(email);
    }
    
    const mergedAccounts: string[][] = [];
    
    for (const [root, emails] of components.entries()) {
        emails.sort();
        mergedAccounts.push([emailToName.get(root)!, ...emails]);
    }
    
    return mergedAccounts;
}
```

### Time Complexity
- Union-Find operations: O(N * K * α(N * K)), where α is the inverse Ackermann function.
- Sorting for each component: O(K log K).
Total: O(N * K log K).

### Space Complexity
- Union-Find data structure: O(N * K).

