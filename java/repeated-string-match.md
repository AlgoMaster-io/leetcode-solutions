## [Leetcode 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimal String Concatenation](#approach-2-optimal-string-concatenation)

---

### Approach 1: Brute Force

#### Intuition
The problem requires us to determine the minimum number of times string `A` has to be repeated such that `B` is a substring of it. A brute force solution involves repeatedly appending `A` to itself until `B` becomes a substring or we surpass a certain reasonable limit.

#### Steps:
1. Start by observing that if the answer exists, the number of times we need to repeat `A` is at least `|B|/|A|` (integer division) and at most `|B|/|A| + 2`. 
2. The reasoning behind the upper bound is that `B` can start appearing mid-way within one of the copies of `A` which might require one or two more copies of `A` to cover whole of `B`.
3. Create a repeated string starting from a single `A`, and keep appending `A` to this string.
4. For each iteration, check if `B` is a substring of the current repeated string.
5. If `B` is found, return the count of repetitions. If after `|B|/|A| + 2` repetitions `B` is not found, return -1.

#### Java Code
```java
public class Solution {
    public int repeatedStringMatch(String A, String B) {
        StringBuilder repeatedA = new StringBuilder();
        int count = 0;
        
        // Repeat A enough times to make it at least as long as B initially
        while (repeatedA.length() < B.length()) {
            repeatedA.append(A);
            count++;
        }
        
        // Check if B is the substring of the current repeated A
        if (repeatedA.toString().contains(B)) {
            return count;
        }
        
        // Append one more A and check again
        repeatedA.append(A);
        count++;
        
        // Final check
        if (repeatedA.toString().contains(B)) {
            return count;
        }
        
        // If not found in both checks, return -1
        return -1;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(N * M), where `N` is the length of `A` and `M` is the length of `B`. This is due to the substring search operation.
- **Space Complexity**: O(N * M), due to repeated string storage.

---

### Approach 2: Optimal String Concatenation

#### Intuition
The brute force solution already works fairly efficiently for most reasonable input sizes. However, we can optimize the repeated string construction and search process slightly using intuitive bounds.

#### Steps:
1. Start by computing the minimum repetitions required using `ceil(|B|/|A|)`, which can be done using `(B.length() + A.length() - 1) / A.length()`.
2. Instead of constructing the repeated string each time from scratch, directly compute this base repeated string.
3. Append one more `A` to handle boundary conditions.
4. Check if `B` is a substring after `minRepeats` and `minRepeats + 1` repetitions and return the respective count if true.
5. Return -1 if `B` is not a substring in both checks.

#### Java Code
```java
public class Solution {
    public int repeatedStringMatch(String A, String B) {
        int minRepeats = (B.length() + A.length() - 1) / A.length();
        StringBuilder repeatedA = new StringBuilder(A.repeat(minRepeats));
        
        if (repeatedA.toString().contains(B)) {
            return minRepeats;
        }
        
        repeatedA.append(A);
        if (repeatedA.toString().contains(B)) {
            return minRepeats + 1;
        }
        
        return -1;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(N + M), where `N` is the length of `A` and `M` is the length of `B`. This comes from building the string and subsequent substring check.
- **Space Complexity**: O(N + M), due to storage of the repeated string. 

This approach optimizes the time taken to build the repeated string for checks.

