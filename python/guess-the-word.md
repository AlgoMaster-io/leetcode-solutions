# [Leetcode 843: Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches:
- [Basic Approach: Random Selection](#random-selection)
- [Optimal Approach: Minimax with Minimax-Solution](#minimax)

## Basic Approach: Random Selection

### Intuition
The basic approach to solve this problem is by making random guesses from the list of provided words until the correct one is found. This approach does not leverage any useful information from previous guesses, which is why it's considered very naive.

### Approach
1. Randomly select a word from `wordlist` to guess.
2. Pass the selected word to the `guess` function which checks how many characters match with the secret word.
3. Remove the guessed word from `wordlist` and repeat the process until the function confirms the correct guess.

### Code
```python
import random

def basic_guess_the_word(wordlist, guess):
    while wordlist:
        # Randomly select a word to guess
        selected_word = random.choice(wordlist)
        
        # Get how many characters matched
        matches = guess(selected_word)
        
        # If all characters match, we break
        if matches == len(selected_word):
            break
        
        # Remove the selected word from wordlist
        wordlist.remove(selected_word)
```

### Complexity Analysis
- **Time Complexity:** O(N^2) in the worst case, where N is the number of words. Each guess might take linear time if guess removes guessed words.
- **Space Complexity:** O(1), no extra space other than input.

## Optimal Approach: Minimax with Minimax-Solution

### Intuition
Instead of randomly guessing a word, we can use a minimax strategy optimized for this problem. The idea is to minimize the maximum number of possible candidates in the wordlist for the next guess. By doing so, we can reduce the number of guesses needed to find the secret word.

### Approach
1. For each word, calculate the number of possible words that could be the secret word if it matches the given number of characters with the guessed word.
2. Choose the word which minimizes the possible candidates after the guess.
3. Only keep words in `wordlist` which have the same number of matching characters as the guessed word.
4. Repeat the process until the correct word is guessed.

### Code
```python
def optimal_guess(wordlist, guess):
    def match(w1, w2):
        """ Calculate how many characters are the same """
        return sum(c1 == c2 for c1, c2 in zip(w1, w2))
    
    while True:
        # Calculate minimax counts for each word
        count_candidates = []
        for word in wordlist:
            count = [0] * 7  # holds up to 6 matches
            for w in wordlist:
                if word != w:
                    matches = match(word, w)
                    count[matches] += 1
            
            # Minimize the maximum number of same-match words
            count_candidates.append((max(count), word))
        
        # Choose the word with minimal maximum number of same-match candidates
        _, best_word = min(count_candidates)
        
        # Guess this best word
        matches = guess(best_word)
        
        if matches == len(best_word):
            break
        
        # Reduce the wordlist to only probable candidates
        wordlist = [w for w in wordlist if match(w, best_word) == matches]
```

### Complexity Analysis
- **Time Complexity:** O(N^2 * M), where N is the number of words and M is the word length. Each iteration through the list checks matches.
- **Space Complexity:** O(1), adjusting in-place without extra space beyond fixed size calculations.

This approach ensures we minimize our chances of making an incorrect guess and quickly narrows down the list of potential secret words.

