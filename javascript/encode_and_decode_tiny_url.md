# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
```javascript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    constructor() {
        this.map = new Map();
        this.id = 0;
    }

    // Encodes a URL to a shortened URL.
    encode(longUrl) {
        this.id++;
        this.map.set(this.id, longUrl);
        return `http://tinyurl.com/${this.id}`; // Return the shortened URL
    }

    // Decodes a shortened URL to its original URL.
    decode(shortUrl) {
        const key = parseInt(shortUrl.replace("http://tinyurl.com/", ""));
        return this.map.get(key); // Retrieve the original URL
    }
}
```

## Approach 2: HashMap with Randomized Key

### Solution
```javascript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    constructor() {
        this.shortToLong = new Map();
        this.longToShort = new Map();
        this.characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    }

    // Generate a random 6-character string
    getRandomKey() {
        let key = '';
        for (let i = 0; i < 6; i++) {
            key += this.characters.charAt(Math.floor(Math.random() * this.characters.length));
        }
        return key;
    }

    // Encodes a URL to a shortened URL.
    encode(longUrl) {
        if (this.longToShort.has(longUrl)) {
            return `http://tinyurl.com/${this.longToShort.get(longUrl)}`;
        }
        let key;
        do {
            key = this.getRandomKey();
        } while (this.shortToLong.has(key)); // Ensure uniqueness of the key
        this.shortToLong.set(key, longUrl);
        this.longToShort.set(longUrl, key);
        return `http://tinyurl.com/${key}`;
    }

    // Decodes a shortened URL to its original URL.
    decode(shortUrl) {
        const key = shortUrl.replace("http://tinyurl.com/", "");
        return this.shortToLong.get(key);
    }
}
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
```javascript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    constructor() {
        this.map = new Map();
        this.characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        this.id = 0;
        this.BASE = this.characters.length;
    }

    // Generate a hash-like key for the URL
    toBase62(id) {
        let encoded = '';
        while (id > 0) {
            encoded = this.characters[id % this.BASE] + encoded;
            id = Math.floor(id / this.BASE);
        }
        return encoded;
    }

    toBase10(base62) {
        let id = 0;
        for (const c of base62) {
            id = id * this.BASE + this.characters.indexOf(c);
        }
        return id;
    }

    // Encodes a URL to a shortened URL.
    encode(longUrl) {
        this.id++;
        const shortKey = this.toBase62(this.id);
        this.map.set(shortKey, longUrl);
        return `http://tinyurl.com/${shortKey}`;
    }

    // Decodes a shortened URL to its original URL.
    decode(shortUrl) {
        const key = shortUrl.replace("http://tinyurl.com/", "");
        return this.map.get(key);
    }
}
```

