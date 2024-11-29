# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
```java
// Time Complexity: O(n * log(k)), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and heap
import java.util.HashMap;
import java.util.PriorityQueue;
import java.util.Map;

public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Step 1: Build a frequency map
        HashMap<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Step 2: Use a min-heap to keep track of the top k elements
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[1], b[1]));

        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            minHeap.add(new int[] { entry.getKey(), entry.getValue() });

            if (minHeap.size() > k) {
                minHeap.poll(); // Remove the element with the smallest frequency
            }
        }

        // Step 3: Extract elements from the heap
        int[] result = new int[k];
        for (int i = 0; i < k; i++) {
            result[i] = minHeap.poll()[0];
        }

        return result;
    }
}
```

## Approach 2: Bucket Sort

### Solution
```java
// Time Complexity: O(n), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and buckets
import java.util.HashMap;
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Step 1: Build a frequency map
        HashMap<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Step 2: Create buckets where index represents frequency
        List<Integer>[] buckets = new List[nums.length + 1];
        for (int key : frequencyMap.keySet()) {
            int frequency = frequencyMap.get(key);
            if (buckets[frequency] == null) {
                buckets[frequency] = new ArrayList<>();
            }
            buckets[frequency].add(key);
        }

        // Step 3: Collect the top k frequent elements
        int[] result = new int[k];
        int index = 0;
        for (int i = buckets.length - 1; i >= 0 && index < k; i--) {
            if (buckets[i] != null) {
                for (int num : buckets[i]) {
                    result[index++] = num;
                    if (index == k) {
                        break;
                    }
                }
            }
        }

        return result;
    }
}
```