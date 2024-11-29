# Sliding Window Median

## Approach 1: Two Heaps

### Solution
java
```java
// Time Complexity: O(n * log(k))
// Space Complexity: O(k)
import java.util.PriorityQueue;
import java.util.Collections;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        double[] medians = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            if (maxHeap.size() == 0 || num <= maxHeap.peek()) {
                maxHeap.add(num);
            } else {
                minHeap.add(num);
            }

            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.add(maxHeap.poll());
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.add(minHeap.poll());
            }

            if (i >= k - 1) {
                if (maxHeap.size() == minHeap.size()) {
                    medians[i - k + 1] = ((double) maxHeap.peek() + minHeap.peek()) / 2;
                } else {
                    medians[i - k + 1] = (double) maxHeap.peek();
                }

                int elementToRemove = nums[i - k + 1];
                if (elementToRemove <= maxHeap.peek()) {
                    maxHeap.remove(elementToRemove);
                } else {
                    minHeap.remove(elementToRemove);
                }

                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.add(maxHeap.poll());
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.add(minHeap.poll());
                }
            }
        }

        return medians;
    }
}
```

c++
```cpp
// Time Complexity: O(n * log(k))
// Space Complexity: O(k)
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        priority_queue<int> maxHeap;
        priority_queue<int, vector<int>, greater<int>> minHeap;
        vector<double> medians(nums.size() - k + 1);

        for (int i = 0; i < nums.size(); ++i) {
            int num = nums[i];
            if (maxHeap.empty() || num <= maxHeap.top()) {
                maxHeap.push(num);
            } else {
                minHeap.push(num);
            }

            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.push(maxHeap.top());
                maxHeap.pop();
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.push(minHeap.top());
                minHeap.pop();
            }

            if (i >= k - 1) {
                if (maxHeap.size() == minHeap.size()) {
                    medians[i - k + 1] = ((double)maxHeap.top() + minHeap.top()) / 2;
                } else {
                    medians[i - k + 1] = (double)maxHeap.top();
                }

                int elementToRemove = nums[i - k + 1];
                if (elementToRemove <= maxHeap.top()) {
                    maxHeap.erase(remove(maxHeap.begin(), maxHeap.end(), elementToRemove), maxHeap.end());
                } else {
                    minHeap.erase(remove(minHeap.begin(), minHeap.end(), elementToRemove), minHeap.end());
                }

                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.push(maxHeap.top());
                    maxHeap.pop();
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.push(minHeap.top());
                    minHeap.pop();
                }
            }
        }

        return medians;
    }
};
```


