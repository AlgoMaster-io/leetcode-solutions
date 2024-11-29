# Sliding Window Median

## Approach 1: Two Heaps (Max Heap and Min Heap)

### Solution
java
```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Collections;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        double[] result = new double[nums.length - k + 1];
        
        for (int i = 0; i < nums.length; i++) {
            if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
                maxHeap.offer(nums[i]);
            } else {
                minHeap.offer(nums[i]);
            }
            
            balanceHeaps(maxHeap, minHeap);
            
            if (i >= k - 1) {
                if (maxHeap.size() > minHeap.size()) {
                    result[i - k + 1] = maxHeap.peek();
                } else {
                    result[i - k + 1] = ((double) maxHeap.peek() + minHeap.peek()) / 2;
                }
                
                int elementToRemove = nums[i - k + 1];
                if (elementToRemove <= maxHeap.peek()) {
                    maxHeap.remove(elementToRemove);
                } else {
                    minHeap.remove(elementToRemove);
                }
                
                balanceHeaps(maxHeap, minHeap);
            }
        }
        
        return result;
    }
    
    private void balanceHeaps(PriorityQueue<Integer> maxHeap, PriorityQueue<Integer> minHeap) {
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.offer(maxHeap.poll());
        } else if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(n log k)
// Space Complexity: O(k)
function medianSlidingWindow(nums, k) {
    let maxHeap = new MinHeap((a, b) => b - a); // Max heap
    let minHeap = new MinHeap((a, b) => a - b); // Min heap
    let result = [];

    for (let i = 0; i < nums.length; i++) {
        if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
            maxHeap.add(nums[i]);
        } else {
            minHeap.add(nums[i]);
        }

        balanceHeaps(maxHeap, minHeap);

        if (i >= k - 1) {
            if (maxHeap.size() > minHeap.size()) {
                result.push(maxHeap.peek());
            } else {
                result.push((maxHeap.peek() + minHeap.peek()) / 2);
            }

            let elementToRemove = nums[i - k + 1];
            if (elementToRemove <= maxHeap.peek()) {
                maxHeap.remove(elementToRemove);
            } else {
                minHeap.remove(elementToRemove);
            }

            balanceHeaps(maxHeap, minHeap);
        }
    }

    return result;
}

function balanceHeaps(maxHeap, minHeap) {
    if (maxHeap.size() > minHeap.size() + 1) {
        minHeap.add(maxHeap.poll());
    } else if (minHeap.size() > maxHeap.size()) {
        maxHeap.add(minHeap.poll());
    }
}

class MinHeap {
    constructor(comparator) {
        this.data = [];
        this.comparator = comparator;
    }
    
    add(value) {
        this.data.push(value);
        this.heapifyUp(this.data.length - 1);
    }
    
    peek() {
        return this.data[0];
    }
    
    poll() {
        if (this.data.length === 1) {
            return this.data.pop();
        }
        const root = this.data[0];
        this.data[0] = this.data.pop();
        this.heapifyDown(0);
        return root;
    }
    
    remove(value) {
        const index = this.data.indexOf(value);
        if (index > -1) {
            this.data[index] = this.data.pop();
            this.heapifyUp(index);
            this.heapifyDown(index);
        }
    }
    
    size() {
        return this.data.length;
    }
    
    isEmpty() {
        return this.data.length === 0;
    }
    
    heapifyUp(index) {
        let parent = Math.floor((index - 1) / 2);
        while (index > 0 && this.comparator(this.data[index], this.data[parent]) < 0) {
            this.swap(index, parent);
            index = parent;
            parent = Math.floor((index - 1) / 2);
        }
    }
    
    heapifyDown(index) {
        let smallest = index;
        const left = index * 2 + 1;
        const right = index * 2 + 2;
        if (left < this.size() && this.comparator(this.data[left], this.data[smallest]) < 0) {
            smallest = left;
        }
        if (right < this.size() && this.comparator(this.data[right], this.data[smallest]) < 0) {
            smallest = right;
        }
        if (smallest !== index) {
            this.swap(index, smallest);
            this.heapifyDown(smallest);
        }
    }
    
    swap(i, j) {
        [this.data[i], this.data[j]] = [this.data[j], this.data[i]];
    }
}
```


