## [Leetcode 535: Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

### Approaches:
1. [Simple Hash Map Solution](#simple-hash-map-solution)
2. [Hashing for Shorter URLs](#hashing-for-shorter-urls)

---

### Simple Hash Map Solution

#### Intuition
The simplest way to approach this problem is by using a hash map. We can map each unique long URL to a unique short URL. To create a short URL, we can simply use a counter that increments with each new URL encoded. This provides a unique identifier for each long URL and can be appended to a base URL to form the short URL.

#### Implementation

```javascript
class Codec {
    constructor() {
        this.urlMap = new Map();  // A map to store the mapping between long URL and short URL
        this.counter = 0;         // A counter to generate unique short URLs
        this.baseURL = "http://tinyurl.com/"; // Base of the short URL
    }
    
    encode(longUrl) {
        // Increment the counter to use as a unique identifier
        this.counter += 1;
        
        // Create the short URL using the counter
        let shortUrl = this.baseURL + this.counter.toString();
        
        // Store the mapping in the map
        this.urlMap.set(shortUrl, longUrl);
        
        // Return the generated short URL
        return shortUrl;
    }
    
    decode(shortUrl) {
        // Retrieve the original long URL given the short URL
        return this.urlMap.get(shortUrl);
    }
}

/**
 * Your functions will be called as such:
 * var codec = new Codec();
 * var encoded = codec.encode(url);
 * var decoded = codec.decode(encoded);
 */
```

#### Complexity Analysis
- **Time Complexity**: O(1) for both encoding and decoding since hash map operations (set and get) are on average O(1).
- **Space Complexity**: O(n), where n is the number of distinct URLs we are encoding and storing in the map.

---

### Hashing for Shorter URLs

#### Intuition
To generate even shorter URLs, we can leverage hashing. By using a hash function, we can generate a shorter and unique identifier for each long URL. This can give even shorter URLs compared to simple counters.

1. Use a fixed-length character space, e.g., alphanumeric characters.
2. For each URL, hash it to generate a fixed-length string using characters from the character space.
3. Ensure the generated hash is unique, possibly handling collisions through rehashing or another resolution technique.

#### Implementation

```javascript
class Codec {
    constructor() {
        this.urlMap = new Map();
        this.baseURL = "http://tinyurl.com/";
        this.chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        this.hashLength = 6;  // Length of the generated hash
    }
    
    generateHash() {
        let hash = '';
        for (let i = 0; i < this.hashLength; i++) {
            const randomIndex = Math.floor(Math.random() * this.chars.length);
            hash += this.chars[randomIndex];
        }
        return hash;
    }
    
    encode(longUrl) {
        let shortUrl;
        
        // Regenerate hash until we find one not yet in use
        do {
            let hash = this.generateHash();
            shortUrl = this.baseURL + hash;
        } while (this.urlMap.has(shortUrl));
        
        // Store the mapping of short URL to long URL
        this.urlMap.set(shortUrl, longUrl);
        
        return shortUrl;
    }
    
    decode(shortUrl) {
        // Retrieve original URL from the map
        return this.urlMap.get(shortUrl);
    }
}

/**
 * Your functions will be called as such:
 * var codec = new Codec();
 * var encoded = codec.encode(url);
 * var decoded = codec.decode(encoded);
 */
```

#### Complexity Analysis
- **Time Complexity**: O(1) for both encoding and decoding, as checking and storing in a map is on average O(1).
- **Space Complexity**: O(n), similar to the first approach, due to storing URLs in the map.

Overall, this hashing method provides much shorter URL encodings compared to using incremental counters and can efficiently manage a large number of URLs.

