# [Leetcode 535: Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approaches
1. [Basic Approach: Utilizing Hash Map](#basic-approach-utilizing-hash-map)
2. [Improved Approach: Shortening URL with Base Characters](#improved-approach-shortening-url-with-base-characters)

### Basic Approach: Utilizing Hash Map

#### Intuition:
The simplest way to encode a URL is to map each URL to an identifier using a hash map. Store this mapping such that every new request to encode a URL gets a unique identifier. To decode, simply use the hash map to retrieve the original URL from its identifier.

#### Approach:
1. Use a dictionary (or Map in TypeScript) to store a mapping from the unique identifier to the original URL.
2. For encoding, assign each URL a unique identifier (incremental integer values or any unique hash).
3. For decoding, use the identifier to fetch the corresponding URL from the dictionary.

#### Code:

```typescript
class Codec {
    private urlToCode = new Map<string, string>();
    private codeToUrl = new Map<string, string>();
    private counter = 0;

    encode(longUrl: string): string {
        // Check if this URL has already been encoded
        if (this.urlToCode.has(longUrl)) {
            return "http://tinyurl.com/" + this.urlToCode.get(longUrl);
        }
        
        // Generate a new unique code
        const code = `code${this.counter++}`;
        this.urlToCode.set(longUrl, code);
        this.codeToUrl.set(code, longUrl);

        return "http://tinyurl.com/" + code;
    }

    decode(shortUrl: string): string {
        // Extract the code from the URL
        const code = shortUrl.replace("http://tinyurl.com/", "");

        // Return the original URL
        return this.codeToUrl.get(code) || "";
    }
}
```

#### Time and Space Complexity:
- **Encode and Decode Time Complexity:** O(1) - Constant time operations for inserting and retrieving from the map.
- **Space Complexity:** O(N) - Space to store N URLs and their respective codes in the map.

---

### Improved Approach: Shortening URL with Base Characters

#### Intuition:
A more space-efficient way is to shorten the identifier using a base conversion (like base62) to reduce the length of the encoded URL. This approach not only shortens the URL but also makes it aesthetically more appealing by using a mix of characters.

#### Approach:
1. Use an array of characters (`a-z`, `A-Z`, `0-9`) to build unique keys.
2. Convert the numeric ID (simple integer counter) using base62 encoding.
3. Store the numeric ID and URL mapping to decode back.

#### Code:

```typescript
class Codec {
    private urlToCode = new Map<string, string>();
    private codeToUrl = new Map<string, string>();
    private counter = 1;

    private chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    encode(longUrl: string): string {
        // Check if this URL has already been encoded
        if (this.urlToCode.has(longUrl)) {
            return "http://tinyurl.com/" + this.urlToCode.get(longUrl);
        }

        // Generate unique code using base62 encoding
        let code = this.toBase62(this.counter++);
        this.urlToCode.set(longUrl, code);
        this.codeToUrl.set(code, longUrl);

        return "http://tinyurl.com/" + code;
    }

    decode(shortUrl: string): string {
        // Extract the code from the URL
        const code = shortUrl.replace("http://tinyurl.com/", "");

        // Return the original URL
        return this.codeToUrl.get(code) || "";
    }

    private toBase62(num: number): string {
        let base62 = "";
        while (num > 0) {
            base62 = this.chars[num % 62] + base62;
            num = Math.floor(num / 62);
        }
        return base62;
    }
}
```

#### Time and Space Complexity:
- **Encode and Decode Time Complexity:** O(1) - Similarly, constant time for lookups and insertions.
- **Space Complexity:** O(N) - Using additional space for character storage is negligible compared to N URLs.

Both approaches serve to solve the problem effectively, with the improved approach providing shorter and more aesthetically pleasing encoded URLs.

