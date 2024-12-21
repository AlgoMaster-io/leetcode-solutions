# [LeetCode Problem 843: Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Minimax Strategy](#minimax-strategy)

---

## Brute Force Approach

### Intuition
In this problem, we have a set of potential secret words (candidates), and we are provided a way to make a guess and get a similarity score (number of matching characters) between the guessed word and the secret word. To solve this problem, we could opt for a brute force approach where we try every word in our candidate list until we find the secret word.

### Steps
1. Start with the full list of words as potential candidates.
2. Pick any random word from the candidates to guess.
3. Based on the match score returned, filter out inconsistent words (those that don't have the same match score with the guessed word as it does with the secret word).
4. Repeat the process with the filtered candidate list until the correct word is guessed.

```csharp
// This is the interface to interact with Guess API
// interface Master {
//     public int Guess(string word);
// }

public class Solution {
    public void FindSecretWord(string[] wordlist, Master master) {
        List<string> candidates = new List<string>(wordlist);
        Random random = new Random();

        int attempts = 10;
        while (attempts > 0) {
            // Randomly select a word to guess from candidates
            string guessWord = candidates[random.Next(candidates.Count)];
            // Make a guess and get match score
            int matchScore = master.Guess(guessWord);
            // Eliminate words from candidates that don't match matchScore
            candidates = candidates.Where(w => MatchScore(guessWord, w) == matchScore).ToList();
            attempts--;
            if (matchScore == 6) {
                // The word was correctly guessed
                return;
            }
        }
    }

    // Helper method to calculate match score
    private int MatchScore(string word1, string word2) {
        int score = 0;
        for (int i = 0; i < word1.Length; i++) {
            if (word1[i] == word2[i]) {
                score++;
            }
        }
        return score;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N * M), where N is the number of words and M is the number of attempts (10 here) since each word is compared against guessed words.
- **Space Complexity**: O(N), where N is the number of words, due to the filtering of candidates.

---

## Minimax Strategy

### Intuition
The brute force approach doesn't prioritize words optimally. Minimax Strategy improves over this by minimizing the potential maximum number of candidates in each step.

### Steps
1. For each word in candidate list, calculate its expected score.
2. The word with the minimum worst-case scenario (i.e., minimized potential remaining candidates) is then chosen for guessing.
3. Repeat the approach until the secret word is found.

```csharp
public class Solution {
    public void FindSecretWord(string[] wordlist, Master master) {
        List<string> candidates = new List<string>(wordlist);
        
        for (int attempts = 0; attempts < 10; attempts++) {
            string guessWord = GetBestGuess(candidates);
            int matchScore = master.Guess(guessWord);
            if (matchScore == 6) return;
            candidates = candidates.Where(w => MatchScore(guessWord, w) == matchScore).ToList();
        }
    }
    
    private string GetBestGuess(List<string> candidates) {
        int leastMaxGroupSize = int.MaxValue;
        string bestWord = candidates[0];

        foreach (string word in candidates) {
            // Score frequency mapping
            int[] scoreFreq = new int[7];
            foreach (string w in candidates) {
                scoreFreq[MatchScore(word, w)]++;
            }

            // Find the maximum group size for this word
            int maxGroupSize = scoreFreq.Max();
            if (maxGroupSize < leastMaxGroupSize) {
                leastMaxGroupSize = maxGroupSize;
                bestWord = word;
            }
        }
        
        return bestWord;
    }

    private int MatchScore(string word1, string word2) {
        int score = 0;
        for (int i = 0; i < 6; i++) {
            if (word1[i] == word2[i]) score++;
        }
        return score;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N^2) in worst-case, due to the double loop when calculating potential scores.
- **Space Complexity**: O(N), similar to the brute force approach, for maintaining the list of candidates.

This Minimax Strategy is much more optimal compared to the simple Brute Force and improves the decision-making process by evaluating the worst-case scenario effects of each potential guess.

