# [Leetcode 843: Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches:
1. [Basic Solution: Random Guessing Strategy](#basic-solution-random-guessing-strategy)
2. [Optimized Solution: Minimizing Worst Case](#optimized-solution-minimizing-worst-case)

### Basic Solution: Random Guessing Strategy

#### Intuition:
The basic strategy involves guessing words from the provided list randomly. We compare how many characters match with the secret word and continue this process until we find the secret word. It's a brute-force approach with zero optimization.

#### Steps:
1. Randomly select a word from the list and guess it.
2. Use the feedback about the number of matching characters to eliminate words that can't be the secret.
3. Repeat until the secret word is guessed.

This approach lacks efficiency since it doesn't actively minimize the number of guesses.

#### Implementation:

```go
// Master interface as described in the problem
type Master struct {
    Secret string
}

func (m *Master) Guess(word string) int {
    matchCount := 0
    for i := 0; i < len(word); i++ {
        if word[i] == m.Secret[i] {
            matchCount++
        }
    }
    return matchCount
}

func findSecretWord(wordlist []string, master *Master) {
    n := len(wordlist)
    for {
        // Randomly select a word from the list and guess
        guess := wordlist[rand.Intn(n)]
        matches := master.Guess(guess)
        if matches == len(guess) {
            return
        }

        // Filter wordlist to remove inconsistent words
        newWordList := []string{}
        for _, word := range wordlist {
            if countMatches(guess, word) == matches {
                newWordList = append(newWordList, word)
            }
        }
        wordlist = newWordList
    }
}

func countMatches(word1, word2 string) int {
    matchCount := 0
    for i := 0; i < len(word1); i++ {
        if word1[i] == word2[i] {
            matchCount++
        }
    }
    return matchCount
}
```

#### Complexity:
- **Time Complexity:** O(N^2), where N is the word list length, due to potentially filtering the list after each guess.
- **Space Complexity:** O(N), required for storing a reduced word list.

### Optimized Solution: Minimizing Worst Case

#### Intuition:
The goal here is to minimize the worst-case number of guesses by choosing words that maximally reduce the search space. The strategy involves selecting words that have the potential to eliminate half of the remaining candidates based on feedback similarity.

#### Steps:
1. Calculate for each word in the list, how many candidates will remain, ensuring a balanced split between known and unknown matches.
2. Select words that maximize the information gained by guessing.
3. Repeat the guessing and filtering process.

By selecting words strategically, we reduce the search space more effectively.

#### Implementation:

```go
func findSecretWord(wordlist []string, master *Master) {
    for attempts := 0; attempts < 10; attempts++ {
        // Count unique words with the same matching letters
        minGuessWord := wordlist[0]
        minGuessCandidates := len(wordlist)

        for _, word := range wordlist {
            counts := map[int]int{}

            for _, w := range wordlist {
                match := countMatches(word, w)
                counts[match]++
            }

            maxCandidates := 0
            for _, count := range counts {
                if count > maxCandidates {
                    maxCandidates = count
                }
            }

            if maxCandidates < minGuessCandidates {
                minGuessCandidates = maxCandidates
                minGuessWord = word
            }
        }

        matches := master.Guess(minGuessWord)

        // Filter word list to only contain potential matches
        newWordList := []string{}
        for _, word := range wordlist {
            if countMatches(minGuessWord, word) == matches {
                newWordList = append(newWordList, word)
            }
        }
        
        wordlist = newWordList
    }
}

func countMatches(word1, word2 string) int {
    matchCount := 0
    for i := 0; i < len(word1); i++ {
        if word1[i] == word2[i] {
            matchCount++
        }
    }
    return matchCount
}
```

#### Complexity:
- **Time Complexity:** O(N^2 * L), where N is the size of the word list and L is the length of each word, due to recalculating matches and repeatedly filtering.
- **Space Complexity:** O(N), required for list manipulation.

Each approach provides a different balance of simplicity and complexity reduction, with the optimized approach offering a more strategic guessing path likely to find the secret word in fewer attempts.

