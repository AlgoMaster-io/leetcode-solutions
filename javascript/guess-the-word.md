# [Leetcode 843: Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches:
- [Approach 1: Random Guessing](#approach-1-random-guessing)
- [Approach 2: Minimax Strategy](#approach-2-minimax-strategy)

## Approach 1: Random Guessing

### Intuition:
The problem involves guessing a secret word through a series of attempts where each attempt is graded by the number of correct characters in the correct position. A straightforward brute force method can be implemented by randomly picking a word from the list and attempting to guess it. Although this might sometimes be lucky, it doesn’t effectively eliminate options and can prove inefficient given worse-case scenarios.

### Steps:
1. Randomly choose a word from the available list and call the `guess` function, which will return the number of matching positions with the secret word.
2. Use the result from the guess to filter words in the list based on matching positions, reducing the potential candidates for the subsequent guess.
3. Repeat the process until you correctly guess the secret word.

### Code:

```javascript
function findSecretWord(wordlist, guess) {
    for (let i = 0; i < 10; i++) {
        // Randomly select a word from the list
        const candidate = wordlist[Math.floor(Math.random() * wordlist.length)];
        // Get the number of matching positions
        const matches = guess(candidate);
        // Filter wordlist based on matching positions
        wordlist = wordlist.filter(word => match(word, candidate) === matches);
    }
}

// Helper function to count matches
function match(word1, word2) {
    let matches = 0;
    for (let i = 0; i < word1.length; i++) {
        if (word1[i] === word2[i]) matches++;
    }
    return matches;
}
```

### Time Complexity:
- This approach has a time complexity of approximately \(O(N \cdot M)\) for filtering in each guessing round, where \(N\) is the number of words and \(M\) is the length of each word.

### Space Complexity:
- Space complexity is \(O(N)\) for storing valid words at any given time.

## Approach 2: Minimax Strategy

### Intuition:
A more strategic approach involves selecting the word in such a way that it minimizes the maximum possible remaining words on each guess. This is achieved using the minimax strategy, which focuses on minimizing the worst-case scenario on the subsequent rounds by choosing a word that maximizes the filtering capability.

### Steps:
1. For each word in the list, calculate how many options it would eliminate in the worst case if it was used for guessing.
2. Choose the word with the minimum worst-case scenario from the possibilities.
3. Filter the list in terms of the previously obtained matching score.
4. Iterate until the secret word is guessed.

### Code:

```javascript
function findSecretWord(wordlist, guess) {
    while (wordlist.length > 0) {
        let count = Array(wordlist.length).fill(0);
        for (let i = 0; i < wordlist.length; i++) {
            for (let j = 0; j < wordlist.length; j++) {
                if (i != j && match(wordlist[i], wordlist[j]) === 0) {
                    count[i]++;
                }
            }
        }
        
        // Word that minimizes 0 matches (maximizing elimination capability in worst-case)
        let minIndex = 0;
        for (let i = 0; i < wordlist.length; i++) {
            if (count[i] < count[minIndex]) {
                minIndex = i;
            }
        }
        
        let matches = guess(wordlist[minIndex]);
        wordlist = wordlist.filter(word => match(word, wordlist[minIndex]) === matches);

        if (matches === 6) break;  // If the guess is correct (all characters match)
    }
}

// Reuse the match function from the previous approach
function match(word1, word2) {
    let matches = 0;
    for (let i = 0; i < word1.length; i++) {
        if (word1[i] === word2[i]) matches++;
    }
    return matches;
}
```

### Time Complexity:
- Approximately \(O(N^2 \cdot M)\), which involves comparing each word against every other word, where \(N\) is the number of words and \(M\) is the length of each word.

### Space Complexity:
- Space complexity remains \(O(N)\), as we primarily operate within the initial list constraints and occasionally reduce its size.  

This approach is more intricate but yields a much higher success rate by minimizing worst-case guesses, ensuring a logical deduction path.

