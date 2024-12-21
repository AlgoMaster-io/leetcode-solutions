# [Leetcode 2353: Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

### Approaches:
- [Approach 1: Naive HashMap and Sorting Approach](#approach-1-naive-hashmap-and-sorting-approach)

## Approach 1: Naive HashMap and Sorting Approach

### Intuition
The problem requires designing a food rating system. For this, the key operations needed are to query and update ratings while maintaining a sorted list of foods by their ratings per cuisine. This can be achieved by using hash structures:

1. **Food to Rating Mapping**: A dictionary that maps each food item to its current rating for fast access and updates.
2. **Food to Cuisine Mapping**: A dictionary that maps each food item to its corresponding cuisine.
3. **Cuisine to Foods Mapping**: A dictionary that maps each cuisine to a list of foods belonging to that cuisine and sort them based on ratings when required.

### Solution

### C# Code
```csharp
public class FoodRatings {
    // Mapping each cuisine to sorted list of food based on ratings
    private Dictionary<string, SortedSet<(int, string)>> cuisineToFoods;
    // Mapping each food to its rating
    private Dictionary<string, int> foodToRating;
    // Mapping each food to its cuisine
    private Dictionary<string, string> foodToCuisine;

    public FoodRatings(string[] foods, string[] cuisines, int[] ratings) {
        cuisineToFoods = new Dictionary<string, SortedSet<(int, string)>>();
        foodToRating = new Dictionary<string, int>();
        foodToCuisine = new Dictionary<string, string>();
        
        for (int i = 0; i < foods.Length; i++) {
            string food = foods[i];
            string cuisine = cuisines[i];
            int rating = ratings[i];

            if (!cuisineToFoods.ContainsKey(cuisine)) {
                cuisineToFoods[cuisine] = new SortedSet<(int, string)>((a, b) => b.Item1 != a.Item1 ? b.Item1.CompareTo(a.Item1) : a.Item2.CompareTo(b.Item2));
            }

            cuisineToFoods[cuisine].Add((rating, food));
            foodToRating[food] = rating;
            foodToCuisine[food] = cuisine;
        }
    }

    public void ChangeRating(string food, int newRating) {
        int oldRating = foodToRating[food];
        string cuisine = foodToCuisine[food];
        
        // Remove the old rating entry
        cuisineToFoods[cuisine].Remove((oldRating, food));
        
        // Add the new rating entry
        cuisineToFoods[cuisine].Add((newRating, food));

        // Update the foodToRating map
        foodToRating[food] = newRating;
    }

    public string HighestRated(string cuisine) {
        // Get the top-rated food for the cuisine
        return cuisineToFoods[cuisine].Min.Item2;
    }
}
```

### Detailed Comments
1. **Initialization (`FoodRatings` Constructor)**:
   - Initialize dictionaries: `foodToRating`, `foodToCuisine`, and `cuisineToFoods`.
   - Populate these dictionaries based on input data.
   - Sort foods within each cuisine based on ratings using a `SortedSet`, ensuring faster insertions and deletions while maintaining order.

2. **Updating (`ChangeRating` Method)**:
   - Remove the existing rating entry for a food item.
   - Update the food's rating.
   - Insert the new rating back into the relevant sorted set for its cuisine.

3. **Querying (`HighestRated` Method)**:
   - Retrieves the food with the highest rating for a given cuisine. Since we've sorted the food items within each cuisine, this operation is efficient.

### Complexity Analysis
- **Time Complexity**:
  - **Initialization**: O(n log n), where n is the number of foods as each insert operation into a `SortedSet` is O(log n).
  - **ChangeRating**: O(log n), as we perform add and remove operations on a `SortedSet`.
  - **HighestRated**: O(1), since we just get the smallest element based on the sorting rule from `SortedSet`.
  
- **Space Complexity**: O(n), where n is the number of foods due to the storage of all foods, ratings, and mapping in dictionaries.

