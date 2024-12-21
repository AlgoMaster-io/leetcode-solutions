# [Leetcode 632: Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap and Two Pointers](#approach-2-min-heap-and-two-pointers)

## Approach 1: Brute Force

### Intuition
The brute force approach involves examining every possible range that contains at least one element from each of the input lists. We can use nested loops to find all such possible ranges. This approach is not optimal but lays the groundwork for understanding the problem.

### Steps
1. Iterate over all elements of lists and take them as potential starting elements of the range.
2. For each starting element, try to extend the range in a way that it includes at least one element from each list.
3. Compare each valid range and keep track of the smallest range found.

### Code
```csharp
public class Solution {
    public int[] SmallestRange(IList<IList<int>> nums) {
        int k = nums.Count;
        int minRange = int.MaxValue;
        int[] result = new int[2];

        foreach (var list in nums) {
            foreach (int start in list) {
                int currentMax = start;
                int currentMin = start;
                bool validRange = True;
                
                // Try to cover elements from each list
                for (int i = 0; i < k; i++) {
                    bool elementFound = false;
                    foreach (int n in nums[i]) {
                        if (n >= start) {
                            currentMax = Math.Max(currentMax, n);
                            currentMin = Math.Min(currentMin, n);
                            elementFound = true;
                            break;
                        }
                    }
                    if (!elementFound) {
                        validRange = false;
                        break;
                    }
                }
                
                if (validRange && currentMax - currentMin < minRange) {
                    minRange = currentMax - currentMin;
                    result[0] = currentMin;
                    result[1] = currentMax;
                }
            }
        }
        
        return result;
    }
}
```

### Complexity Analysis
- Time Complexity: \(O(n^2 \times k)\) - where \(n\) is the total number of elements across all lists and \(k\) is the number of lists.
- Space Complexity: \(O(1)\) - No extra space other than variables.

## Approach 2: Min-Heap and Two Pointers

### Intuition
The optimal approach uses a Min-Heap (Priority Queue) to efficiently maintain the current minimum range which covers elements from each list. The heap helps efficiently extract the smallest element, while maintaining a window by adding the next element from that list to which the smallest element belonged.

### Steps
1. Initialize the Min-Heap with the first element of each list and a pointer to track next elements.
2. Track maximum of current elements across the lists.
3. Extract the minimum element from the heap, which indicates the current smallest element in the range.
4. Check if current range is the smallest range.
5. Move the pointer in the list from which the minimum element was extracted and add the next element to the heap.
6. Update the maximum if the new element is bigger.
7. Repeat until we reach the end of any list which makes it possible that the range contains at least one from each list.

### Code
```csharp
public class Solution {
    public int[] SmallestRange(IList<IList<int>> nums) {
        int k = nums.Count;
        PriorityQueue<Element, int> minHeap = new PriorityQueue<Element, int>();
        int max = int.MinValue;
        int start = 0, end = int.MaxValue;
        
        // Initialize priority queue with the first element of each list
        for (int i = 0; i < k; i++) {
            int elem = nums[i][0];
            max = Math.Max(max, elem);
            minHeap.Enqueue(new Element(i, 0, elem), elem);
        }
        
        while (true) {
            Element minElem = minHeap.Dequeue();
            
            // Update smallest range
            if (max - minElem.Value < end - start) {
                start = minElem.Value;
                end = max;
            }
            
            if (minElem.Index + 1 == nums[minElem.ListIndex].Count) {
                break; // We reached the end of one list
            }
            
            // Move to the next element in the current list
            int nextValue = nums[minElem.ListIndex][minElem.Index + 1];
            max = Math.Max(max, nextValue);
            minHeap.Enqueue(new Element(minElem.ListIndex, minElem.Index + 1, nextValue), nextValue);
        }
        
        return new int[] { start, end };
    }
    
    class Element {
        public int ListIndex { get; }
        public int Index { get; }
        public int Value { get; }
        
        public Element(int listIndex, int index, int value) {
            ListIndex = listIndex;
            Index = index;
            Value = value;
        }
    }
}
```

### Complexity Analysis
- Time Complexity: \(O(n \log k)\) - where \(n\) is the total number of elements across all lists and \(k\) is the number of lists. Each operations of the heap takes logarithmic time.
- Space Complexity: \(O(k)\) - For the Min-Heap which stores one element from each list at all times.

