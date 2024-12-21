# [843. Guess the Word](https://leetcode.com/problems/guess-the-word/)

## Approaches
- [Basic Approach: Random Guess](#random-guess)
- [Intermediate Approach: Minimax Strategy](#minimax-strategy)
- [Optimal Approach: Data Filtering with Candidate Pool Reduction](#optimal-data-filtering)

### Random Guess

#### Intuition
The simplest solution involves randomly guessing words from the word list. This approach lacks strategic guessing but doesn't require complicated logic. The only check is to ensure you don't guess the same word twice.

#### Steps
1. Randomly select a word from the list of available words.
2. Call the `guess` function with the selected random word.
3. Continue guessing until the correct word is guessed (i.e., `guess` returns the word length).

#### Code
```java
public void findSecretWord(String[] wordlist, Master master) {
    Random rand = new Random();
    for (int attempt = 0; attempt < 10; attempt++) {
        // Choose a random index and use that word to make a guess
        String candidate = wordlist[rand.nextInt(wordlist.length)];
        int matches = master.guess(candidate);
        if (matches == candidate.length) return; // Guessed correctly
    }
}
```

#### Complexity

- **Time Complexity**: O(N), where N is the number of attempts (in this method, 10 maximum).
- **Space Complexity**: O(1), as no additional data structures are needed.

### Minimax Strategy

#### Intuition
A more informed approach is to use minimax to reduce the candidate pool aggressively by minimizing the maximum number of words that could remain after a guess. This strategy leads to fewer guesses by attempting to maximize information gain.

#### Steps
1. Count matches for each possible word against all others.
2. Choose a word that, on average, splits the possibility space well.
3. Use guessed similarity (number of letter matches) to filter the list.
4. Repeat until the correct word is guessed.

#### Code
```java
public void findSecretWord(String[] wordlist, Master master) {
    int[][] hamming = new int[6][6];
    for (int guess = 0; guess < 10; guess++) {
        Map<String, Integer> count = new HashMap<>();
        
        // Calculate similarity by number of matches.
        for (String word1 : wordlist) {
            for (String word2 : wordlist) {
                if (matches(word1, word2) == 0) continue;
                count.put(word1, count.getOrDefault(word1, 0) + 1);
            }
        }
        
        String candidate = wordlist[0];
        int minWordsLeft = count.containsKey(candidate) ? count.get(candidate) : 0;
        
        // Choose candidate with highest minimum elimination.
        for (String word : wordlist) {
            int minWordEffect = count.getOrDefault(word, 0);
            if (minWordEffect <= minWordsLeft) {
                candidate = word;
                minWordsLeft = minWordEffect;
            }
        }
        
        int matches = master.guess(candidate);
        List<String> newList = new ArrayList<>();
        for (String word : wordlist) {
            if (matches(candidate, word) == matches) newList.add(word);
        }
        wordlist = newList.toArray(new String[newList.size()]);
    }
}

private int matches(String a, String b) {
    int matchCount = 0;
    for (int i = 0; i < a.length(); i++) {
        if (a.charAt(i) == b.charAt(i)) matchCount++;
    }
    return matchCount;
}
```

#### Complexity
- **Time Complexity**: O(N^2 * L), where N is the number of words and L is the length of each word.
- **Space Complexity**: O(N), maintaining a filtered list of candidate words.


### Optimal Data Filtering

#### Intuition
Incorporating chosen strategic guessing along with data filtering reduces unnecessary guesses significantly. The idea is to reduce candidate words as much as possible each turn.

#### Steps
1. For each guess, calculate the number of matching letters for every pair of words.
2. Optimize the next guess based on its ability to partition the wordpool efficiently.
3. Break the loop when guessed successfully.

#### Code
```java
public void findSecretWord(String[] wordlist, Master master) {
    int[][] similarity = new int[wordlist.length][wordlist.length];
    for (int i = 0; i < wordlist.length; i++) {
        for (int j = 0; j < wordlist.length; j++) {
            similarity[i][j] = matches(wordlist[i], wordlist[j]);
        }
    }
    
    List<String> candidates = new ArrayList<>(Arrays.asList(wordlist));
    while (!candidates.isEmpty() && candidates.size() > 0) {
        int tryIndex = 0;
        int minMaxSize = candidates.size();
        for (int i = 0; i < candidates.size(); i++) {
            int[] groups = new int[7];
            for (String s : candidates) {
                groups[matches(candidates.get(i), s)]++;
            }
            int maxSize = Arrays.stream(groups).max().getAsInt();
            if (maxSize < minMaxSize) {
                minMaxSize = maxSize;
                tryIndex = i;
            }
        }
        
        String result = candidates.get(tryIndex);
        int similarityScore = master.guess(result);
        if (similarityScore == 6) return;
        
        List<String> nextRound = new ArrayList<>();
        for (String s : candidates) {
            if (matches(s, result) == similarityScore) {
                nextRound.add(s);
            }
        }
        candidates = nextRound;
    }
}

private int matches(String a, String b) {
    int matchCount = 0;
    for (int i = 0; i < a.length(); i++) {
        if (a.charAt(i) == b.charAt(i)) {
            matchCount++;
        }
    }
    return matchCount;
}
```

#### Complexity
- **Time Complexity**: O(N^2 * L), due to double iteration over the word list.
- **Space Complexity**: O(N), due to copied lists of candidate words.

