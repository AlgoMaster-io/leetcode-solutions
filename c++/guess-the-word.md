# [Leetcode 843: Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches

- [Approach 1: Random Guessing with Feedback](#approach-1-random-guessing-with-feedback)
- [Approach 2: Minimax Strategy with Feedback](#approach-2-minimax-strategy-with-feedback)

## Approach 1: Random Guessing with Feedback

### Intuition:
The problem involves guessing a hidden word in as few attempts as possible with feedback on how many characters are matching. A simple approach is to randomly select a word from the list of possible words and use the feedback to filter down potential candidates. While this approach is more of a brute-force and may take many attempts, it can help us understand the problem space.

### Steps:
1. Randomly guess a word from the list.
2. Use the feedback (number of matching characters) to filter out candidates that could not possibly be the word (i.e., words that have a different number of matching characters compared to the guessed word).
3. Repeat until the correct word is guessed.

### Code:

```cpp
#include <vector>
#include <string>
#include <cstdlib>
#include <ctime>

using namespace std;

// This interface is provided by the problem statement.
class Master {
public:
    int guess(string word);
};

class Solution {
public:
    void findSecretWord(vector<string>& wordlist, Master& master) {
        srand(time(0));  // Initialize random seed
        for (int i = 0; i < 10; ++i) {  // We are allowed at most 10 guesses
            int idx = rand() % wordlist.size();  // Select a random index
            string guessWord = wordlist[idx];
            int matches = master.guess(guessWord);
            if (matches == 6) return;  // Correct guess

            vector<string> filteredWordlist;
            for (auto& word : wordlist) {
                if (matchCount(word, guessWord) == matches) {
                    filteredWordlist.push_back(word);
                }
            }
            wordlist = filteredWordlist;
        }
    }

private:
    int matchCount(const string& a, const string& b) {
        int count = 0;
        for (int i = 0; i < a.size(); ++i) {
            if (a[i] == b[i]) count++;
        }
        return count;
    }
};
```

### Time Complexity:
- Worst-case: O(N) for each guess due to filtering, where N is the number of words.
- In the outer loop, the worst-case scenario would involve utilizing almost all 10 guesses.

### Space Complexity:
- O(N) for storing potential candidates.

---

## Approach 2: Minimax Strategy with Feedback

### Intuition:
Improving on the random guessing, we can employ a minimax strategy to minimize the maximum possible loss (i.e., select guesses such that the number of remaining candidates is minimized in the worst possible scenario).

### Steps:
1. For each word, calculate how many words it would partition the list into based on match counts.
2. Choose the word that minimizes the maximum size of any partition when guessed.
3. Use the guess feedback to reduce the list of potential words.
4. Repeat until the word is guessed.

### Code:

```cpp
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

// This interface is provided by the problem statement.
class Master {
public:
    int guess(string word);
};

class Solution {
public:
    void findSecretWord(vector<string>& wordlist, Master& master) {
        for (int i = 0; i < 10; ++i) {  // We are allowed at most 10 guesses
            string guessWord = findBestWord(wordlist);
            int matches = master.guess(guessWord);
            if (matches == 6) return;  // Correct guess

            vector<string> filteredWordlist;
            for (auto& word : wordlist) {
                if (matchCount(word, guessWord) == matches) {
                    filteredWordlist.push_back(word);
                }
            }
            wordlist = filteredWordlist;
        }
    }

private:
    int matchCount(const string& a, const string& b) {
        int count = 0;
        for (int i = 0; i < a.size(); ++i) {
            if (a[i] == b[i]) count++;
        }
        return count;
    }

    string findBestWord(vector<string>& wordlist) {
        int minMaxSize = wordlist.size();
        string bestWord;
        
        for (auto& word1 : wordlist) {
            unordered_map<int, int> countMap;
            for (auto& word2 : wordlist) {
                int matches = matchCount(word1, word2);
                countMap[matches]++;
            }
            int maxSize = 0;
            for (auto& [key, value] : countMap) {
                if (value > maxSize) {
                    maxSize = value;
                }
            }
            if (maxSize < minMaxSize) {
                minMaxSize = maxSize;
                bestWord = word1;
            }
        }
        return bestWord;
    }
};
```

### Time Complexity:
- Each guess involves calculating match scores for each pair of words, resulting in O(N^2) operations per guess where N is the number of words.
- Given potential for 10 guesses, the complexity stays at O(N^2).

### Space Complexity:
- O(N) for storing intermediate data structures for matching counts and candidate lists.

Both approaches aim to efficiently narrow down the guess list, but the minimax strategy is significantly more optimized in terms of reducing potential incorrect paths.

