# Koko Eating Bananas
Koko loves to eat bananas. There are n piles of bananas, and the i-th pile has piles[i] bananas. The guards have gone, and Koko has h hours to eat all the bananas. Each hour, she chooses a pile of bananas and eats k bananas from that pile. If the pile has fewer than k bananas, she eats all of them instead, and there will be no bananas left in that pile for the next hour.

Return the minimum integer k such that Koko can eat all the bananas within h hours.
### Constraints:
- 1 <= piles.length <= 10^4
- piles.length <= h <= 10^9
- 1 <= piles[i] <= 10^9

### Examples
```javascript
Input: piles = [3,6,7,11], h = 8
Output: 4

Input: piles = [30,11,23,4,20], h = 5
Output: 30

Input: piles = [30,11,23,4,20], h = 6
Output: 23
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
In a brute-force approach, we can iterate over all possible eating speeds k from 1 to the maximum number of bananas in the largest pile, checking for each speed if Koko can eat all the bananas in h hours. However, this approach is inefficient because it involves checking many values of k and can lead to a time complexity of O(n * max(piles)), where n is the number of piles.

Steps:
1. Iterate through possible values of k starting from 1.
2. For each k, simulate the process to see if Koko can finish eating all piles within h hours.
3. Return the smallest k that satisfies the condition.
##### Time Complexity:
O(n * max(piles)), where n is the number of piles and max(piles) is the largest pile size.
##### Space Complexity:
O(1), because we only use a few variables to track the possible values of k and time.
##### Python Code:
```python
def can_eat_in_time(piles, h, k):
    hours = 0
    for pile in piles:
        hours += (pile + k - 1) // k  # Equivalent to ceil(pile / k)
    return hours <= h

def minEatingSpeed(piles, h):
    for k in range(1, max(piles) + 1):
        if can_eat_in_time(piles, h, k):
            return k
```
### Approach 2: Binary Search on Eating Speed (Optimal Solution)
##### Intuition: 
We can use binary search to efficiently find the minimum possible eating speed k. The possible values for k range from 1 to the largest pile size in piles. The idea is to apply binary search over this range to minimize k while ensuring that Koko can finish all bananas within h hours.

- Lower bound (low): The smallest possible speed is 1 banana per hour.
- Upper bound (high): The maximum possible speed is the size of the largest pile (max(piles)), since Koko can finish the largest pile in one hour at this speed.

Steps:
1. Initialize low = 1 and high = max(piles).
2. Perform binary search:
   - Calculate the middle point mid = (low + high) // 2.
   - Check if Koko can finish all the bananas at speed mid using the helper function can_eat_in_time(piles, h, mid).
   - If she can finish, update high = mid to try smaller values.
   - If she cannot finish, update low = mid + 1 to increase the eating speed.
3. The binary search will converge, and low will be the minimum speed k.
##### Time Complexity:
O(n log max(piles)), where n is the number of piles, and log max(piles) comes from the binary search.
##### Space Complexity:
O(1), because we only use a few variables for binary search and time calculations.
##### Python Code:
```python
def can_eat_in_time(piles, h, k):
    hours = 0
    for pile in piles:
        hours += (pile + k - 1) // k  # Equivalent to ceil(pile / k)
    return hours <= h

def minEatingSpeed(piles, h):
    # Binary search on the possible values of k
    low, high = 1, max(piles)
    
    while low < high:
        mid = (low + high) // 2
        if can_eat_in_time(piles, h, mid):
            high = mid  # Try for smaller k
        else:
            low = mid + 1  # Increase k
    
    return low
```
### Explanation of Code:
- Binary Search:
The binary search starts with a range of possible eating speeds from 1 to max(piles). We progressively narrow down this range by checking the midpoint (mid) using the can_eat_in_time function, which simulates the process of eating bananas at speed mid.

- Helper Function can_eat_in_time(piles, h, k):
This function calculates how many hours it would take Koko to finish eating all the piles if she eats k bananas per hour. For each pile, the time taken is ceil(pile / k), which is computed as (pile + k - 1) // k (this avoids using floating-point division).

- Binary Search Condition:
If Koko can finish eating all bananas at speed mid, we reduce the search space to find a potentially smaller value of k. Otherwise, we increase the speed and continue searching.
### Edge Cases:
1. Single Pile: If there is only one pile, the eating speed is equal to the size of the pile if h == 1. Otherwise, it will depend on how many hours Koko has.
2. Many Small Piles, Few Hours: When there are many small piles but few hours, the binary search should quickly find the largest k needed to eat all piles in the limited time.
3. Large Pile, Many Hours: If there's a very large pile and many hours, Koko can eat slowly, so the binary search should return a small k.
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n * max(piles))      | O(1)             |
| Binary Search (Optimal)	     | 	O(n log max(piles))            | O(1)             |

The Binary Search approach is the most efficient, reducing the time complexity to O(n log max(piles)) while using constant space.