# [843. Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches
1. [Approach 1: Randomized Guess](#approach-1-randomized-guess)
2. [Approach 2: Minimax Strategy](#approach-2-minimax-strategy)

### Approach 1: Randomized Guess

The problem at its core requires guessing the secret word in as few attempts as possible. A simple approach is to randomly guess words until we find one that matches. This is not efficient but satisfies the problem constraints.

- **Intuition**: 
  - Randomly select a word from the list.
  - Use the result of the guess (number of matching characters) to filter out words that could not possibly be the secret word.
  - Repeat this process with the reduced list until the correct word is found or attempts are exhausted.

- **Time Complexity**: O(N * M), where N is the number of words and M is the word length.  
- **Space Complexity**: O(N).

```typescript
/**
 * Interface for the provided Master API.
 */
interface Master {
  guess(word: string): number;
}

function findSecretWord(wordlist: string[], master: Master): void {
  const wordLength = wordlist[0].length;

  // Helper function to count matching characters
  function match(a: string, b: string): number {
    let matches = 0;
    for (let i = 0; i < wordLength; i++) {
      if (a[i] === b[i]) {
        matches++;
      }
    }
    return matches;
  }

  let tries = 10;
  while (tries > 0) {
    const index = Math.floor(Math.random() * wordlist.length);
    const guessWord = wordlist[index];
    const matches = master.guess(guessWord);

    // If the word matches completely, we are done
    if (matches === wordLength) return;

    // Filter words based on the number of matches
    const filteredList = wordlist.filter(word => match(word, guessWord) === matches);
    wordlist = filteredList;
    tries--;
  }
}
```

### Approach 2: Minimax Strategy

For a more strategic approach, we can utilize a minimax strategy. This involves minimizing the maximum regret, by selecting the word which leaves us with the most balanced set of possibilities, thus potentially reducing the average number of guesses required.

- **Intuition**: 
  - For each word, simulate guessing it and count how many words would remain for each possible match count.
  - Select the word which results in the minimal maximum number of remaining possibilities.
  
- **Time Complexity**: O(N^2 * M), due to comparing each word with all others.  
- **Space Complexity**: O(N).

```typescript
function findSecretWord(wordlist: string[], master: Master): void {
  const wordLength = wordlist[0].length;

  // Helper function to count matching characters
  function match(a: string, b: string): number {
    let matches = 0;
    for (let i = 0; i < wordLength; i++) {
      if (a[i] === b[i]) {
        matches++;
      }
    }
    return matches;
  }

  for (let attempts = 0; attempts < 10; attempts++) {
    // Calculate match counts for each word
    const countMap = new Map<string, number>();
    for (let i = 0; i < wordlist.length; i++) {
      countMap.set(wordlist[i], 0);
      const countArray = Array(wordLength + 1).fill(0);

      for (let j = 0; j < wordlist.length; j++) {
        if (i !== j) {
          const matches = match(wordlist[i], wordlist[j]);
          countArray[matches]++;
        }
      }

      const worstCase = Math.max(...countArray);
      countMap.set(wordlist[i], worstCase);
    }

    // Select word with minimal worst-case
    let minWord = wordlist[0];
    let minWorstCase = Infinity;
    for (const [word, worstCase] of countMap.entries()) {
      if (worstCase < minWorstCase) {
        minWorstCase = worstCase;
        minWord = word;
      }
    }

    // Guess the word and update wordlist based on result
    const x = master.guess(minWord);

    if (x === wordLength) return;

    wordlist = wordlist.filter(word => match(word, minWord) === x);
  }
}
```

In these approaches, the idea is to strategically reduce the set of potential secret words by leveraging the match count provided by the `guess(word)` function, and thereby zero in on the correct word efficiently.

