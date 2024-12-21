
# [Leetcode 752: Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approaches
1. [Breadth-First Search (BFS) Approach](#approach-1)
2. [Bi-Directional Breadth-First Search (Bi-BFS) Approach](#approach-2)

---

### Approach 1: Breadth-First Search (BFS) Approach

#### Intuition:
To solve the "Open the Lock" problem, we can use a BFS approach. The lock can be represented as a graph where each state (combination of numbers) is a node, and each valid turn of the wheel represents an edge to a new node. BFS is suited here as it explores the shallowest nodes first (minimum number of turns to reach a state).

We initialize a queue starting with the initial lock position ("0000") and attempt to reach the target position while avoiding deadends which act as restricted nodes in our graph.

#### Steps:
1. Use a queue to explore node combinations starting from "0000".
2. Use a set for the deadends to avoid unnecessary processing.
3. Use a set for visited nodes to prevent revisiting states.
4. For each move, update each wheel by increasing or decreasing the digit by one.
5. Count the number of steps taken to reach the target configuration.

#### Code:
```java
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;

public class OpenLock {

    public int openLock(String[] deadends, String target) {
        Set<String> deadSet = new HashSet<>();
        for (String d : deadends) {
            deadSet.add(d);
        }

        if (deadSet.contains("0000")) return -1;
        if (target.equals("0000")) return 0;

        Queue<String> queue = new LinkedList<>();
        queue.offer("0000");
        Set<String> visited = new HashSet<>();
        visited.add("0000");

        int depth = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            // Process all nodes at current depth
            for (int i = 0; i < size; i++) {
                String node = queue.poll();
                if (node.equals(target)) return depth;

                // Try all possible movements
                for (String next : getNextStates(node)) {
                    if (!visited.contains(next) && !deadSet.contains(next)) {
                        queue.offer(next);
                        visited.add(next);
                    }
                }
            }
            depth++;
        }

        return -1; // Not possible to reach the target
    }

    private List<String> getNextStates(String current) {
        List<String> states = new ArrayList<>();
        char[] chars = current.toCharArray();
        
        for (int i = 0; i < 4; i++) {
            char original = chars[i];
            // Move wheel forward
            chars[i] = original == '9' ? '0' : (char)(original + 1);
            states.add(new String(chars));

            // Move wheel backward
            chars[i] = original == '0' ? '9' : (char)(original - 1);
            states.add(new String(chars));

            chars[i] = original; // Restore original digit
        }
        
        return states;
    }
}
```

#### Complexity:
- **Time Complexity**: \(O(n \cdot 10^4)\), where \(n\) is the length of string (fixed as 4). This accounts for generating all possible states.
- **Space Complexity**: \(O(10^4)\) for the queue and visited set.

---

### Approach 2: Bi-Directional Breadth-First Search (Bi-BFS) Approach

#### Intuition:
A more efficient way to solve this problem is using bidirectional BFS. Instead of exploring from just the start to the target, we concurrently explore from both directions—the start position and the target position—converging in the middle. This significantly reduces the number of states we need to process.

#### Steps:
1. Maintain two queues: one starting from "0000" and one from the target.
2. Alternate the exploration from each front, ensuring we always process from the smaller queue, optimizing the breadth exploration.
3. If the two fronts meet, we have found the shortest path.

#### Code:
```java
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;

public class OpenLockBiBFS {

    public int openLock(String[] deadends, String target) {
        Set<String> deadSet = new HashSet<>();
        for (String d : deadends) {
            deadSet.add(d);
        }

        if (deadSet.contains("0000")) return -1;
        if (target.equals("0000")) return 0;

        Set<String> startSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        startSet.add("0000");
        endSet.add(target);

        Set<String> visited = new HashSet<>();
        visited.add("0000");
        visited.add(target);

        int depth = 0;

        // BFS from both start and end
        while (!startSet.isEmpty() && !endSet.isEmpty()) {
            // Always extend smaller queue for optimization
            if (startSet.size() > endSet.size()) {
                Set<String> temp = startSet;
                startSet = endSet;
                endSet = temp;
            }

            Set<String> next = new HashSet<>();
            for (String current : startSet) {
                if (endSet.contains(current)) {
                    return depth;
                }
                if (deadSet.contains(current)) {
                    continue;
                }
                for (String neighbor : getNextStates(current)) {
                    if (!visited.contains(neighbor)) {
                        visited.add(neighbor);
                        next.add(neighbor);
                    }
                }
            }
            startSet = next;
            depth++;
        }

        return -1; // Not possible to reach the target
    }

    private List<String> getNextStates(String current) {
        List<String> states = new ArrayList<>();
        char[] chars = current.toCharArray();
        
        for (int i = 0; i < 4; i++) {
            char original = chars[i];
            // Move wheel forward
            chars[i] = original == '9' ? '0' : (char)(original + 1);
            states.add(new String(chars));

            // Move wheel backward
            chars[i] = original == '0' ? '9' : (char)(original - 1);
            states.add(new String(chars));

            chars[i] = original; // Restore original digit
        }
        
        return states;
    }
}
```

#### Complexity:
- **Time Complexity**: \(O(b^{d/2})\), where \(b\) is the branching factor and \(d\) is the depth of the tree. The bidirectional approach cuts the required state exploration significantly.
- **Space Complexity**: \(O(b^{d/2})\) for maintaining the two fronts in the bidirectional search.

Both approaches provide viable methods to solve the "Open the Lock" problem, with the second one optimized for larger deadend sets and more complex target values.

