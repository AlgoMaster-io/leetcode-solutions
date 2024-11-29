# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
```java
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap
import java.util.PriorityQueue;

class KthLargest {
    private PriorityQueue<Integer> minHeap;
    private int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        minHeap = new PriorityQueue<>(k); // Min-heap to store the k largest elements

        // Add all elements to the heap and maintain only k elements
        for (int num : nums) {
            add(num);
        }
    }

    public int add(int val) {
        minHeap.add(val); // Add the new value to the heap

        // Remove the smallest element if the heap exceeds size k
        if (minHeap.size() > k) {
            minHeap.poll();
        }

        return minHeap.peek(); // The kth largest element is the root of the heap
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```