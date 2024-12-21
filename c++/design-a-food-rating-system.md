## [2353. Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

In this problem, we need to design a system that supports adding foods along with their ratings and cuisines and allows updating ratings and querying the highest-rated food in a specific cuisine.

### Table of Contents
- [Approach 1: Naive Approach using Brute Force Searching](#approach-1)
- [Approach 2: Two-way Mappings with Sorted Data Structures](#approach-2)

---

## Approach 1: Naive Approach using Brute Force Searching

### Intuition
This approach involves storing all food items in a list and using a brute-force search to find the highest-rated food in a specific cuisine. This is straightforward but inefficient for large datasets.

### Implementation

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

using namespace std;

class FoodRatings {
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for (int i = 0; i < foods.size(); i++) {
            foodInfo[foods[i]] = {cuisines[i], ratings[i]};
            foodsByCuisine[cuisines[i]].push_back(foods[i]);
        }
    }
    
    void changeRating(string food, int newRating) {
        foodInfo[food].second = newRating;
    }
    
    string highestRated(string cuisine) {
        int maxRating = INT_MIN;
        string bestFood;
        for (const string& food : foodsByCuisine[cuisine]) {
            if (foodInfo[food].second > maxRating || 
                (foodInfo[food].second == maxRating && food < bestFood)) {
                maxRating = foodInfo[food].second;
                bestFood = food;
            }
        }
        return bestFood;
    }

private:
    unordered_map<string, pair<string, int>> foodInfo;
    unordered_map<string, vector<string>> foodsByCuisine;
};
```

### Time and Space Complexity
- **Time Complexity:** 
  - `changeRating`: O(1)
  - `highestRated`: O(n) where n is the number of foods in a given cuisine.
- **Space Complexity:** O(n) where n is the total number of foods.

---

## Approach 2: Two-way Mappings with Sorted Data Structures

### Intuition
Utilizing a more sophisticated structure where each cuisine keeps its foods in a priority queue (or sorted data structure) to efficiently maintain and query the highest-rated food. With this, we can achieve optimal querying for the highest-rated food.

### Implementation

```cpp
#include <iostream>
#include <unordered_map>
#include <set>
#include <vector>
#include <string>

using namespace std;

class FoodRatings {
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for (int i = 0; i < foods.size(); i++) {
            foodInfo[foods[i]] = {cuisines[i], ratings[i]};
            foodSets[cuisines[i]].emplace(ratings[i], foods[i]);
        }
    }

    void changeRating(string food, int newRating) {
        auto &info = foodInfo[food];
        auto &cuisineSet = foodSets[info.first];
        cuisineSet.erase({info.second, food});
        info.second = newRating;
        cuisineSet.emplace(newRating, food);
    }
    
    string highestRated(string cuisine) {
        return foodSets[cuisine].rbegin()->second;
    }

private:
    unordered_map<string, pair<string, int>> foodInfo;
    unordered_map<string, set<pair<int, string>>> foodSets;
};
```

### Time and Space Complexity
- **Time Complexity:**
  - `changeRating`: O(log n), which comes from the operations on the set for each cuisine.
  - `highestRated`: O(1) for querying the top element in the set.
- **Space Complexity:** O(n) for storing the foods and their ratings.

This approach offers much better efficiency in scenarios where frequent queries are made for the highest-rated food. Use of data structures like `std::set` helps maintain order while providing fast insert/delete operations.

