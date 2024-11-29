# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(1)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result(nums1.size());

        // Iterate over each element in nums1
        for (int i = 0; i < nums1.size(); i++) {
            int current = nums1[i];
            int indexInNums2 = -1;

            // Find the index of current in nums2
            for (int j = 0; j < nums2.size(); j++) {
                if (nums2[j] == current) {
                    indexInNums2 = j;
                    break;
                }
            }

            // Look for the next greater element to the right in nums2
            result[i] = -1; // Default if no greater element is found
            for (int j = indexInNums2 + 1; j < nums2.size(); j++) {
                if (nums2[j] > current) {
                    result[i] = nums2[j];
                    break;
                }
            }
        }
        return result;
    }
};
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n + m)
// Space Complexity: O(m)
#include <vector>
#include <unordered_map>
#include <stack>
using namespace std;

class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        // Map to store the next greater element for each number in nums2
        unordered_map<int, int> nextGreaterMap;
        stack<int> stack;

        // Traverse nums2 in reverse to populate the nextGreaterMap
        for (int num : nums2) {
            // Maintain the stack in decreasing order
            while (!stack.empty() && stack.top() <= num) {
                stack.pop();
            }
            // If stack is not empty, the top of the stack is the next greater element
            nextGreaterMap[num] = stack.empty() ? -1 : stack.top();
            stack.push(num);
        }

        // Build the result for nums1 using the map
        vector<int> result(nums1.size());
        for (int i = 0; i < nums1.size(); i++) {
            result[i] = nextGreaterMap[nums1[i]];
        }

        return result;
    }
};
```

