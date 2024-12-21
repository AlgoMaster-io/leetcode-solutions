# [Leetcode 218: The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Sorting and Priority Queue](#sorting-and-priority-queue)

### Brute Force Approach

#### Intuition:
The brute force approach involves going through each building and marking its influence on the skyline. This approach considers the height of each building across its span on the x-axis and updates a height array. The result skyline is derived from transitions in this height array.

Here's how this approach is systematically handled:
- Create an array to hold the maximum height at every x-coordinate encountered from any building.
- Iterate from the minimum x-coordinate to the maximum x-coordinate, and calculate the height at each position.
- Construct the skyline from the transitions in height.

#### Time Complexity:
- **O(n \cdot W)**, where n is the number of buildings and W is the width of the spread of buildings across the x-axis.
  
#### Space Complexity:
- **O(W)**, where W is the width covered by buildings.

#### Implementation:

```csharp
public class Solution {
    public IList<IList<int>> GetSkyline(int[][] buildings) {
        if (buildings == null || buildings.Length == 0) return new List<IList<int>>();

        // To track the height at each point
        int[] heightMap = new int[buildings.Max(b => b[1]) + 1];
        foreach (var building in buildings) {
            int left = building[0];
            int right = building[1];
            int height = building[2];
            for (int i = left; i < right; i++) {
                heightMap[i] = Math.Max(heightMap[i], height);
            }
        }

        // Construct the skyline
        List<IList<int>> result = new List<IList<int>>();
        int previousHeight = 0;
        for (int i = 0; i < heightMap.Length; i++) {
            if (heightMap[i] != previousHeight) {
                result.Add(new List<int>{i, heightMap[i]});
                previousHeight = heightMap[i];
            }
        }
        return result;
    }
}
```

### Sorting and Priority Queue

#### Intuition:
The optimal solution uses a combination of sorting and a priority queue (max heap) to efficiently determine the skyline:
- Transform each building into two events: the start and the end.
- Sort these events. Starts are processed before ends, and ties are broken by the height in a way that guarantees correctness.
- Use a priority queue to keep track of active buildings and quickly determine the current skyline's height.
- As we process each event, we update the queue and determine if a change in the skyline occurs.

#### Time Complexity:
- **O(n log n)**, due to sorting and priority queue operations.

#### Space Complexity:
- **O(n)**, mainly for the storage used by the priority queue to keep track of live buildings.

#### Implementation:

```csharp
public class Solution {
    public IList<IList<int>> GetSkyline(int[][] buildings) {
        List<Tuple<int, int>> events = new List<Tuple<int, int>>();

        // Break down buildings into start and end events
        foreach (var building in buildings) {
            events.Add(Tuple.Create(building[0], -building[2])); // start event
            events.Add(Tuple.Create(building[1], building[2]));  // end event
        }
        
        // Sort events based on their x coordinate and height
        events.Sort((a, b) => 
            a.Item1 == b.Item1 ? 
            a.Item2.CompareTo(b.Item2) : 
            a.Item1.CompareTo(b.Item1));

        SortedDictionary<int, int> activeHeights = new SortedDictionary<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        activeHeights[0] = 1;  // Add a base height

        List<IList<int>> result = new List<IList<int>>();
        int previousMaxHeight = 0;

        foreach (var e in events) {
            int x = e.Item1;
            int h = e.Item2;
            
            if (h < 0) { // start of a building
                if (activeHeights.ContainsKey(-h)) {
                    activeHeights[-h]++;
                } else {
                    activeHeights[-h] = 1;
                }
            } else { // end of a building
                if (activeHeights.ContainsKey(h)) {
                    if (activeHeights[h] == 1) {
                        activeHeights.Remove(h);
                    } else {
                        activeHeights[h]--;
                    }
                }
            }

            // Current max height from active buildings, it's current skyline height
            int currentMaxHeight = activeHeights.Keys.First();
            
            // If the max height has changed, add the coordinate to the result
            if (previousMaxHeight != currentMaxHeight) {
                result.Add(new List<int> { x, currentMaxHeight });
                previousMaxHeight = currentMaxHeight;
            }
        }
        
        return result;
    }
}
```

In the optimal solution, by treating start and end of buildings as events and attentively tracking the active set of building heights with a max-heap, transitions in the skyline are naturally highlighted, facilitating an efficient derivation of the skyline profile.

