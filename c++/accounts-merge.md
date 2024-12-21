# [721. Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approaches
1. [Basic Approach: Union-Find / Disjoint Set with Adjacency List](#basic-approach-union-find-disjoint-set-with-adjacency-list)
2. [Optimal Approach: Union-Find with Email Mapping](#optimal-approach-union-find-with-email-mapping)

---

### Basic Approach: Union-Find / Disjoint Set with Adjacency List

**Intuition:**

The problem can be visualized as a graph where each account corresponds to a node and edges exist between accounts if they share common emails. The task is to identify connected components in the graph which translates to merging the accounts. We can utilize the Union-Find (or Disjoint Set Union) to perform this efficiently.

**Steps:**

1. **Create Union-Find Data Structure**: 
   - Initialize roots and rank lists. The roots list helps identify the parent/leader of each email component, while the rank is used for union by rank optimization.

2. **Build the Graph (Union emails)**: 
   - For each account, union the emails with the first email and assign a parent. This step ensures all emails in an account belong to the same component.

3. **Find Connected Components**: 
   - For each email, determine the connected component it belongs to using the `find` operation.

4. **Collect Emails for Each Root**:
   - Traverse the emails, using the root found, collect all emails under the same root.

5. **Construct Result**: 
   - For each root, collect and sort emails and generate account information.

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <set>
#include <algorithm>

class UnionFind {
public:
    UnionFind(int n) : root(n), rank(n, 1) {
        for (int i = 0; i < n; ++i) {
            root[i] = i;
        }
    }

    int find(int x) {
        if (x != root[x]) {
            root[x] = find(root[x]); // path compression
        }
        return root[x];
    }

    void unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    }

private:
    std::vector<int> root;
    std::vector<int> rank;
};

std::vector<std::vector<std::string>> accountsMerge(std::vector<std::vector<std::string>>& accounts) {
    std::unordered_map<std::string, int> emailToIndex;
    int n = accounts.size();
    UnionFind uf(n);

    // Map each email to an index and union them for each account
    for (int i = 0; i < n; ++i) {
        for (int j = 1; j < accounts[i].size(); ++j) {
            if (!emailToIndex.count(accounts[i][j])) {
                emailToIndex[accounts[i][j]] = i;
            } else {
                uf.unionSet(i, emailToIndex[accounts[i][j]]);
            }
        }
    }

    std::unordered_map<int, std::set<std::string>> indexToEmails;
    for (const auto& [email, index] : emailToIndex) {
        int root = uf.find(index);
        indexToEmails[root].insert(email);
    }

    std::vector<std::vector<std::string>> mergedAccounts;
    for (const auto& [index, emails] : indexToEmails) {
        std::vector<std::string> account(emails.begin(), emails.end());
        account.insert(account.begin(), accounts[index][0]);
        mergedAccounts.push_back(account);
    }

    return mergedAccounts;
}
```

**Time Complexity:** O(N * M * logN), where N is the number of accounts and M is the average number of emails per account. The logN term comes from path compression in the union-find operations.

**Space Complexity:** O(N * M), for storing email mappings and union-find structures.

---

### Optimal Approach: Union-Find with Email Mapping

**Intuition:**

In this approach, we use a direct mapping of emails to indices and utilize union-find to merge accounts. Furthermore, advanced optimizations for union operations allow us to process emails efficiently, resulting in faster account linkage.

**Steps:**

1. **Union-Find Initialization**:
   - Use a Union-Find data structure. Every email corresponds to a node.

2. **Mapping Email to Account**:
   - Map each email to its account using a dictionary.

3. **Union Operation**:
   - For each account, union the emails with the earlier encountered email in the account list, ensuring all emails in an account map to a single parent.

4. **Collect Email Sets**:
   - Traverse all emails, using find to determine the component and group emails under their root node.

5. **Format Output**:
   - Sort emails within each account and format the account details for the final output.

```cpp
#include <unordered_map>
#include <unordered_set>
#include <vector>
#include <string>
#include <algorithm>

class UnionFindEfficiency {
public:
    std::unordered_map<std::string, std::string> parent;

    std::string find(std::string s) {
        if (parent[s] != s) {
            parent[s] = find(parent[s]); // Path compression
        }
        return parent[s];
    }

    void unionSet(std::string a, std::string b) {
        std::string rootA = find(a);
        std::string rootB = find(b);
        if (rootA != rootB) {
            parent[rootB] = rootA;
        }
    }
};

std::vector<std::vector<std::string>> accountsMergeOptimal(std::vector<std::vector<std::string>>& accounts) {
    UnionFindEfficiency uf;
    std::unordered_map<std::string, std::string> emailToName;

    // Initial union find setup and mapping emails to the user
    for (const auto& account : accounts) {
        for (int i = 1; i < account.size(); ++i) {
            uf.parent[account[i]] = account[i];
            emailToName[account[i]] = account[0];
        }
    }

    for (const auto& account : accounts) {
        std::string rootEmail = account[1];
        for (int i = 2; i < account.size(); ++i) {
            uf.unionSet(rootEmail, account[i]);
        }
    }

    std::unordered_map<std::string, std::set<std::string>> rootToEmails;
    for (const auto& [email, _] : emailToName) {
        std::string root = uf.find(email);
        rootToEmails[root].insert(email);
    }

    std::vector<std::vector<std::string>> mergedAccounts;
    for (const auto& [root, emails] : rootToEmails) {
        std::vector<std::string> account{emailToName[root]};
        account.insert(account.end(), emails.begin(), emails.end());
        mergedAccounts.push_back(account);
    }

    return mergedAccounts;
}
```

**Time Complexity:** O(N * M * α(N * M)), where N is the number of accounts and M is the average number of emails per account. The α function represents the inverse Ackermann function, very slowly growing.

**Space Complexity:** O(N * M), for managing mappings and union-find data structures.

Both solutions fundamentally rely on the idea of associating emails in distinct accounts to identify and merge connected components efficiently. Understanding the union-find methodology is crucial for grasping the optimal merging process.

