# [Leetcode 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

* [Approach 1: Basic Approach](#approach-1)
* [Approach 2: Incrementally Extend `A` to At Least `B`](#approach-2)
* [Approach 3: Concatenate `A` a Calculated Number of Times](#approach-3)

## Approach 1: Basic Approach

### Intuition
The basic idea is to repeat the string `A` multiple times and check if the repeated string contains string `B` as a substring. This approach generally involves repeated appending and substring operations which might not be the most efficient.

### Algorithm
1. Start with a string `repeatedA` initialized as `A`.
2. Repeat `A` and append it to `repeatedA` while the length of `repeatedA` is less than the length of `B`.
3. For each repetition, check if `B` is a substring of `repeatedA`.
4. If found, return the number of times `A` was repeated.
5. If `repeatedA` surpasses twice the length of `B` without finding `B`, return -1.

### Implementation
```cpp
#include <iostream>

bool containsSubstr(const std::string& repeatedA, const std::string& B) {
    return repeatedA.find(B) != std::string::npos;
}

int repeatedStringMatch(std::string A, std::string B) {
    std::string repeatedA = A;
    int count = 1;

    // Increase the length of repeatedA until its length is at least the length of B
    while (repeatedA.length() < B.length()) {
        repeatedA += A;
        count++;
    }

    // Check if B is a substring of repeatedA. If True, return the count
    if (containsSubstr(repeatedA, B)) return count;
    
    // Append one more A and check again
    repeatedA += A;
    if (containsSubstr(repeatedA, B)) return count + 1;

    return -1;
}

int main() {
    std::string A = "abcd";
    std::string B = "cdabcdab";
    std::cout << repeatedStringMatch(A, B) << std::endl;  // Output: 3
    return 0;
}
```

### Complexity
- **Time Complexity:** O(N * M) where N is the length of `A` and M is the length of `B`. It requires potentially scanning the repeatedA string up to twice the length of `B`.
- **Space Complexity:** O(N * M) due to the potentially large repeatedA string storage.

## Approach 2: Incrementally Extend `A` to At Least `B`

### Intuition
Instead of naively extending `A`, we can repeat `A` only until its length is just enough to potentially contain `B`. We only need to check up to three times the length of `A`.

### Algorithm
1. Calculate the minimum number of repetitions needed so that the length of `A` is at least as long as `B`.
2. Check up to two more additional repetitions of `A`.
3. If `B` is not a substring by then, return -1.

### Implementation
```cpp
#include <iostream>
#include <cmath>

bool containsSubstr(const std::string& repeatedA, const std::string& B) {
    return repeatedA.find(B) != std::string::npos;
}

int repeatedStringMatch(std::string A, std::string B) {
    int minRepeats = std::ceil((float) B.length() / A.length());
    std::string repeatedA;
    
    for (int i = 0; i < minRepeats; ++i) {
        repeatedA += A;
    }
    
    // Check if B is a substring of the current repeatedA
    if (containsSubstr(repeatedA, B)) return minRepeats;
    
    // Sometimes one extra append might be required
    repeatedA += A;
    if (containsSubstr(repeatedA, B)) return minRepeats + 1;

    return -1;
}

int main() {
    std::string A = "ab";
    std::string B = "abababa";
    std::cout << repeatedStringMatch(A, B) << std::endl;  // Output: 4
    return 0;
}
```

### Complexity
- **Time Complexity:** O(N * M), where N is the length of `A` and M is the length of `B`.
- **Space Complexity:** O(N + M) to store repeated strings.

## Approach 3: Concatenate `A` a Calculated Number of Times

### Intuition
The most optimal way is to cut down repetitive checks by calculating exactly how many times `A` must be repeated to fully encompass `B`. You only need to check repeated concatenations of `A` equal to `ceil(M/N)` and one more beyond.

### Algorithm
1. Calculate `repeats = ceil(B.length() / A.length())`.
2. Check the substring existence in `repeatedA` for `repeats` and `repeats + 1` times of `A`.
3. Return the minimum repetitions if `B` is a substring, else return -1.

### Implementation
```cpp
#include <iostream>
#include <string>

bool containsSubstr(const std::string& repeatedA, const std::string& B) {
    return repeatedA.find(B) != std::string::npos;
}

int repeatedStringMatch(std::string A, std::string B) {
    int repeats = std::ceil((float) B.length() / A.length());
    std::string repeatedA;
    
    for (int i = 0; i < repeats; ++i) {
        repeatedA += A;
    }
    
    if (containsSubstr(repeatedA, B)) return repeats;
    
    repeatedA += A; // One extra repetition for checking edges.
    if (containsSubstr(repeatedA, B)) return repeats + 1;

    return -1;
}

int main() {
    std::string A = "a";
    std::string B = "aa";
    std::cout << repeatedStringMatch(A, B) << std::endl;  // Output: 2
    return 0;
}
```

### Complexity
- **Time Complexity:** O(N + M) by minimizing the number of concatenations and by using efficient substring search.
- **Space Complexity:** O(N + M) for the constructed string containing the repetitions of `A`.

