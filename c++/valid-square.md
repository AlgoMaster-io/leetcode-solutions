# [Leetcode 593: Valid Square](https://leetcode.com/problems/valid-square/)

## Solutions:
1. [Basic Approach: Using Distance Formula and Brute Force](#solution-1)
2. [Optimized Approach: Using Sorted Distances](#solution-2)

### Solution 1: Basic Approach - Using Distance Formula and Brute Force

#### Intuition
To determine whether four points form a square, we can start by verifying if all sides of a square are equal and the diagonals are equal as well. In Euclidean space, the distance between two points \((x_1, y_1)\) and \((x_2, y_2)\) is given by the formula:

\[ d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2} \]

We'll calculate all pairwise distances and check if we get four equal sides and two equal diagonals.

#### Steps
1. Calculate the distance between each pair of points.
2. Use a set to store these distances. A valid square should have exactly 2 distinct distances: one for the side and one for the diagonal.
3. The set should contain exactly two different values and the larger one must appear exactly twice (representing the diagonals).

#### Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_set>

using namespace std;

// Function to calculate squared distance between two points
int squaredDistance(vector<int>& p1, vector<int>& p2) {
    return (p2[0] - p1[0]) * (p2[0] - p1[0]) + (p2[1] - p1[1]) * (p2[1] - p1[1]);
}

// Function to check if four points can form a valid square
bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
    vector<int> points[] = {p1, p2, p3, p4};
    unordered_set<int> distances;
    
    // Compute all pairwise distances
    for (int i = 0; i < 4; ++i) {
        for (int j = i + 1; j < 4; ++j) {
            int dist = squaredDistance(points[i], points[j]);
            if (dist == 0) return false; // To handle the case of overlapping points
            distances.insert(dist);
        }
    }
    
    // Valid square must have exactly 2 different non-zero distances
    return distances.size() == 2;
}

int main() {
    vector<int> p1 = {0, 0};
    vector<int> p2 = {1, 1};
    vector<int> p3 = {1, 0};
    vector<int> p4 = {0, 1};
    cout << validSquare(p1, p2, p3, p4) << endl;  // Output: 1 (true)
    return 0;
}
```

#### Time and Space Complexities
- **Time Complexity**: O(1). We are doing a fixed number of comparisons (specifically, calculating 6 distances).
- **Space Complexity**: O(1). We are using a fixed amount of extra space.

### Solution 2: Optimized Approach - Using Sorted Distances

#### Intuition
A more structured approach involves sorting the squared distances, so we can directly compare and ensure we have a correct sequence of 4 equal sides and 2 equal diagonals.

#### Steps
1. Compute all 6 pairwise squared distances.
2. Sort these distances.
3. We expect the first four to be equal (representing the four sides) and the last two to be equal (representing the diagonals).
4. Additionally, ensure the diagonal's length is greater than the side's length.

#### Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Function to compute squared distance between two points
int squaredDistance(vector<int>& p1, vector<int>& p2) {
    return (p2[0] - p1[0]) * (p2[0] - p1[0]) + (p2[1] - p1[1]) * (p2[1] - p1[1]);
}

// Check if the provided points form a valid square
bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
    vector<int> points[] = {p1, p2, p3, p4};
    vector<int> distances;
    
    // Compute all pairwise distances
    for (int i = 0; i < 4; ++i) {
        for (int j = i + 1; j < 4; ++j) {
            int dist = squaredDistance(points[i], points[j]);
            distances.push_back(dist);
        }
    }
    
    // Sort the distances
    sort(distances.begin(), distances.end());
    
    // Check the conditions for a valid square
    return distances[0] > 0 &&          // Ensure no overlapping points
           distances[0] == distances[1] &&
           distances[1] == distances[2] &&
           distances[2] == distances[3] &&
           distances[4] == distances[5] &&
           distances[3] < distances[4]; // Ensure that the diagonals are longer than sides
}

int main() {
    vector<int> p1 = {0, 0};
    vector<int> p2 = {1, 1};
    vector<int> p3 = {1, 0};
    vector<int> p4 = {0, 1};
    cout << validSquare(p1, p2, p3, p4) << endl;  // Output: 1 (true)
    return 0;
}
```

#### Time and Space Complexities
- **Time Complexity**: O(1). This approach also has a fixed number of operations, including computation and sorting of 6 distances.
- **Space Complexity**: O(1). We use constant space to store distances. 

Both solutions are efficient due to their constant operation count relative to the problem's fixed input size (i.e., four points).

