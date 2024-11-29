# 721. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Approach 1: Union-Find

### Solution
```cpp
// Time Complexity: O(A * logA), where A is the total number of emails
// Space Complexity: O(A)

#include <vector>
#include <string>
#include <unordered_map>
#include <map>
#include <set>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        // Map each email to a parent email
        unordered_map<string, string> parent;
        // Map to retrieve the name associated with each email
        unordered_map<string, string> owner;
        
        // Initialize parent to self and store owner for each email
        for (auto& account : accounts) {
            string ownerName = account[0];
            for (int i = 1; i < account.size(); i++) {
                parent[account[i]] = account[i]; // Each email is its own parent initially
                owner[account[i]] = ownerName; // Record owner of each email
            }
        }
        
        // Union-Find: Union the first email with the rest in each account
        for (auto& account : accounts) {
            string firstEmail = find(account[1], parent);
            for (int i = 2; i < account.size(); i++) {
                string nextEmail = find(account[i], parent);
                parent[nextEmail] = firstEmail;
            }
        }
        
        // Group emails by their root parent
        unordered_map<string, set<string>> unionedEmails;
        for (auto& p : parent) {
            string email = p.first;
            string rootEmail = find(email, parent);
            unionedEmails[rootEmail].insert(email);
        }
        
        // Construct result using owners and grouped emails
        vector<vector<string>> result;
        for (auto& p : unionedEmails) {
            vector<string> emails(p.second.begin(), p.second.end());
            emails.insert(emails.begin(), owner[p.first]); // Add owner to the start
            result.push_back(emails);
        }
        
        return result;
    }

private:
    // Helper method to find the root of an email
    string find(string email, unordered_map<string, string>& parent) {
        if (parent[email] != email) {
            parent[email] = find(parent[email], parent); // Path compression
        }
        return parent[email];
    }
};
```

## Approach 2: DFS to Group Emails

### Solution
```cpp
// Time Complexity: O(A * N), where A is the total number of emails, and N is the average number of emails in an account
// Space Complexity: O(A)

#include <vector>
#include <string>
#include <unordered_map>
#include <unordered_set>
#include <map>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        unordered_map<string, string> emailToName;
        unordered_map<string, vector<string>> graph;
        
        // Build graph and map email to owner
        for (auto& account : accounts) {
            string name = account[0];
            for (int i = 1; i < account.size(); i++) {
                emailToName[account[i]] = name;
                graph[account[i]]; // Ensure email is in graph
                if (i == 1) continue;
                // Create undirected connections
                graph[account[i - 1]].push_back(account[i]);
                graph[account[i]].push_back(account[i - 1]);
            }
        }
        
        // Traverse graph to merge accounts
        unordered_set<string> visited;
        vector<vector<string>> result;
        
        for (auto& p : emailToName) {
            string email = p.first;
            if (visited.find(email) == visited.end()) {
                vector<string> list;
                dfs(email, graph, visited, list);
                sort(list.begin(), list.end());
                list.insert(list.begin(), emailToName[email]); // Add owner to the start
                result.push_back(list);
            }
        }
        
        return result;
    }
    
private:
    // Helper method for DFS traversal
    void dfs(string email, unordered_map<string, vector<string>>& graph, unordered_set<string>& visited, vector<string>& list) {
        visited.insert(email);
        list.push_back(email);
        for (string neighbor : graph[email]) {
            if (visited.find(neighbor) == visited.end()) {
                dfs(neighbor, graph, visited, list);
            }
        }
    }
};
```

