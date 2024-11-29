# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
typescript
```typescript
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)
class Solution {
    accountsMerge(accounts: string[][]): string[][] {
        const parent: Map<string, string> = new Map();
        const owner: Map<string, string> = new Map();
        
        // Initialize parent to self and store owner for each email
        for (const account of accounts) {
            const ownerName = account[0];
            for (let i = 1; i < account.length; i++) {
                parent.set(account[i], account[i]); // Each email is its own parent initially
                owner.set(account[i], ownerName); // Record owner of each email
            }
        }
        
        // Union-Find: Union the first email with the rest in each account
        for (const account of accounts) {
            const firstEmail = this.find(account[1], parent);
            for (let i = 2; i < account.length; i++) {
                const nextEmail = this.find(account[i], parent);
                parent.set(nextEmail, firstEmail);
            }
        }
        
        // Group emails by their root parent
        const unionedEmails: Map<string, Set<string>> = new Map();
        for (const email of parent.keys()) {
            const rootEmail = this.find(email, parent);
            if (!unionedEmails.has(rootEmail)) {
                unionedEmails.set(rootEmail, new Set());
            }
            unionedEmails.get(rootEmail)!.add(email);
        }
        
        // Construct result using owners and grouped emails
        const result: string[][] = [];
        for (const rootEmail of unionedEmails.keys()) {
            const emails = Array.from(unionedEmails.get(rootEmail)!);
            emails.sort(); // Sort emails lexographically
            emails.unshift(owner.get(rootEmail)!); // Add owner to the start
            result.push(emails);
        }

        return result;
    }
    
    // Helper method to find the root of an email
    private find(email: string, parent: Map<string, string>): string {
        if (parent.get(email) !== email) {
            parent.set(email, this.find(parent.get(email)!, parent)); // Path compression
        }
        return parent.get(email)!;
    }
}
```

## Approach 2: DFS to Group Emails

### Solution
typescript
```typescript
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)
class Solution {
    accountsMerge(accounts: string[][]): string[][] {
        const emailToName: Map<string, string> = new Map();
        const graph: Map<string, string[]> = new Map();
        
        // Build graph and map email to owner
        for (const account of accounts) {
            const name = account[0];
            for (let i = 1; i < account.length; i++) {
                emailToName.set(account[i], name);
                if (!graph.has(account[i])) {
                    graph.set(account[i], []);
                }
                
                if (i === 1) continue;
                // Create undirected connections
                graph.get(account[i - 1])!.push(account[i]);
                graph.get(account[i])!.push(account[i - 1]);
            }
        }
        
        // Traverse graph to merge accounts
        const visited: Set<string> = new Set();
        const result: string[][] = [];
        
        for (const email of emailToName.keys()) {
            if (!visited.has(email)) {
                const list: string[] = [];
                this.dfs(email, graph, visited, list);
                list.sort(); // Sort emails lexographically
                list.unshift(emailToName.get(email)!); // Add owner to the start
                result.push(list);
            }
        }
        
        return result;
    }
    
    // Helper method for DFS traversal
    private dfs(email: string, graph: Map<string, string[]>, visited: Set<string>, list: string[]): void {
        visited.add(email);
        list.push(email);
        for (const neighbor of graph.get(email) || []) {
            if (!visited.has(neighbor)) {
                this.dfs(neighbor, graph, visited, list);
            }
        }
    }
}
```

