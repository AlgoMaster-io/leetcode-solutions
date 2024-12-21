# [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approaches:
- [Approach 1: Basic Brute Force Approach](#approach-1-basic-brute-force-approach)
- [Approach 2: Using Trie (Prefix Tree)](#approach-2-using-trie-prefix-tree)

---

## Approach 1: Basic Brute Force Approach

### Intuition
The simplest way to handle the `addWord` method is to store the words in a list (or array). For the `search` method, if the search word contains any dots ('.'), we need to match each possible character for each dot. This makes it necessary to check against every word in the list.

### Code
```javascript
class WordDictionary {
  constructor() {
    this.words = [];
  }

  // Adds a word into the data structure
  addWord(word) {
    this.words.push(word);
  }

  // Searches for the word in the data structure
  search(word) {
    for (let w of this.words) {
      if (this._match(word, w)) return true;
    }
    return false;
  }

  // Function to match two words, one with '.' wildcard
  _match(word, candidate) {
    if (word.length !== candidate.length) return false;
    for (let i = 0; i < word.length; i++) {
      if (word[i] !== '.' && word[i] !== candidate[i]) {
        return false;
      }
    }
    return true;
  }
}

```

### Complexity Analysis
- **Time Complexity**: 
  - `addWord`: O(1) per addition since we just append to the array.
  - `search`: O(n * m) where n is the number of words and m is the length of the `search` word because each word in the list has to be matched character-by-character.
- **Space Complexity**: O(n) for storing the words, where n is the number of words added.

---

## Approach 2: Using Trie (Prefix Tree)

### Intuition
A trie is a tree-like data structure that stores words by their common prefix. By using a trie, we can handle both the `addWord` and `search` operations more efficiently, especially for handling the `'.'` wildcard. The search can be done in a recursive manner, considering all possible characters for each dot.

### Code
```javascript
class WordDictionary {
  constructor() {
    this.trie = {};
  }

  // Adds a word into the data structure
  addWord(word) {
    let node = this.trie;
    for (let char of word) {
      if (!node[char]) node[char] = {}; // Create node if it doesn't exist
      node = node[char];
    }
    node.isEndOfWord = true; // Mark the end of a word
  }

  // Searches for the word in the data structure
  search(word) {
    const searchInNode = (node, i) => {
      if (i === word.length) return node.isEndOfWord === true;

      const char = word[i];
      if (char === '.') {
        // If char is '.', check all possible nodes at this level
        for (let key in node) {
          if (key !== 'isEndOfWord' && searchInNode(node[key], i + 1)) {
            return true;
          }
        }
        return false;
      } else {
        if (!node[char]) return false; // If char does not exist
        return searchInNode(node[char], i + 1);
      }
    };

    return searchInNode(this.trie, 0);
  }
}
```

### Complexity Analysis
- **Time Complexity**: 
  - `addWord`: O(m), where m is the length of the word, since each character of the word is processed once.
  - `search`: O(m) for searching without '.' and can be up to O(26^m) in the worst case with all '.' (though practically less due to constraints).
- **Space Complexity**: Proportional to the total number of characters stored, with an overhead for each node in the trie. This would be approximately O(n * m) assuming average word length m and n words.

---

By utilizing a trie, the solution becomes more efficient in terms of both time and space as compared to the brute force method, especially when `.` wildcards are sparsely used.

