# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
typescript
```typescript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    private map: Map<number, string> = new Map();
    private id: number = 0;

    // Encodes a URL to a shortened URL.
    public encode(longUrl: string): string {
        this.id++;
        this.map.set(this.id, longUrl);
        return `http://tinyurl.com/${this.id}`; // Return the shortened URL
    }

    // Decodes a shortened URL to its original URL.
    public decode(shortUrl: string): string {
        const key = parseInt(shortUrl.replace("http://tinyurl.com/", ""), 10);
        return this.map.get(key) || ""; // Retrieve the original URL
    }
}
```

## Approach 2: HashMap with Randomized Key

### Solution
typescript
```typescript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    private shortToLong: Map<string, string> = new Map();
    private longToShort: Map<string, string> = new Map();
    private readonly characters: string = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    // Generate a random 6-character string
    private getRandomKey(): string {
        let key = '';
        for (let i = 0; i < 6; i++) {
            key += this.characters.charAt(Math.floor(Math.random() * this.characters.length));
        }
        return key;
    }

    // Encodes a URL to a shortened URL.
    public encode(longUrl: string): string {
        if (this.longToShort.has(longUrl)) {
            return `http://tinyurl.com/${this.longToShort.get(longUrl)}`;
        }
        let key: string;
        do {
            key = this.getRandomKey();
        } while (this.shortToLong.has(key)); // Ensure uniqueness of the key
        this.shortToLong.set(key, longUrl);
        this.longToShort.set(longUrl, key);
        return `http://tinyurl.com/${key}`;
    }

    // Decodes a shortened URL to its original URL.
    public decode(shortUrl: string): string {
        const key = shortUrl.replace("http://tinyurl.com/", "");
        return this.shortToLong.get(key) || "";
    }
}
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
typescript
```typescript
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
class Codec {
    private map: Map<string, string> = new Map();
    private readonly characters: string = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private readonly BASE: number = this.characters.length;
    private id: number = 0;

    // Generate a hash-like key for the URL
    private toBase62(id: number): string {
        let encoded = '';
        while (id > 0) {
            encoded = this.characters.charAt(id % this.BASE) + encoded;
            id = Math.floor(id / this.BASE);
        }
        return encoded;
    }

    private toBase10(base62: string): number {
        let id = 0;
        for (let i = 0; i < base62.length; i++) {
            id = id * this.BASE + this.characters.indexOf(base62.charAt(i));
        }
        return id;
    }

    // Encodes a URL to a shortened URL.
    public encode(longUrl: string): string {
        this.id++;
        const shortKey = this.toBase62(this.id);
        this.map.set(shortKey, longUrl);
        return `http://tinyurl.com/${shortKey}`;
    }

    // Decodes a shortened URL to its original URL.
    public decode(shortUrl: string): string {
        const key = shortUrl.replace("http://tinyurl.com/", "");
        return this.map.get(key) || "";
    }
}
```

