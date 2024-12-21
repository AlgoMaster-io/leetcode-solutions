## [134. Gas Station](https://leetcode.com/problems/gas-station/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Greedy Approach](#greedy-approach)

## Brute Force Approach

### Intuition
The problem requires us to determine a starting gas station index such that we can complete the circuit. The brute force approach suggests testing all possible starting points and checking if the circuit can be completed starting at each of those points.

### Steps:
1. Start from each station, and keep track of the `remaining gas`.
2. Move to the next station by consuming gas based on the respective cost.
3. If at any station the `remaining gas` becomes negative, that starting point is not feasible.
4. If you can complete the circuit, return the starting index.
5. If no station can serve as a starting point, return `-1`.

### Code
```python
def canCompleteCircuit(gas, cost):
    n = len(gas)
    for start in range(n):
        fuel = 0
        valid = True
        for i in range(n):
            index = (start + i) % n  # Check circularly
            fuel += gas[index] - cost[index]
            if fuel < 0:
                valid = False
                break
        if valid:
            return start
    return -1
```

### Time Complexity:
- **O(n^2)**: Each starting point is checked which requires another round checking of `n` stations.

### Space Complexity:
- **O(1)**: No extra space is utilized, just a few variables.

## Greedy Approach

### Intuition
To improve efficiency, we leverage the fact that if there's a solution, the total gas provided is greater or equal to the cost, and the more comprehensive condition for valid start involves tracking aggregate surpluses and deficits.

### Steps:
1. Traverse the gas stations to calculate total_surplus and current_surplus.
2. If at any station the `current_surplus` is negative, it is not possible to reach this station from the start. Thus, the next station becomes our new starting point.
3. Simultaneously, if the total surplus at the end is non-negative, the last recorded starting point is valid. Otherwise, it isn't possible.

### Code
```python
def canCompleteCircuit(gas, cost):
    total_surplus = 0      # Total gas surplus
    current_surplus = 0    # Surplus until current station
    start_station = 0

    for i in range(len(gas)):
        net = gas[i] - cost[i]
        total_surplus += net
        current_surplus += net
        
        # If current surplus is negative, reset the starting point
        if current_surplus < 0:
            current_surplus = 0
            start_station = i + 1

    return start_station if total_surplus >= 0 else -1
```

### Time Complexity:
- **O(n)**: We make a single pass over the input arrays.

### Space Complexity:
- **O(1)**: Only a couple of variables are used for tracking data.

By applying the Greedy approach, we efficiently determine the best starting station, should it exist, in linear time. This contrast to the brute force method allows handling much larger inputs effectively.

