# [Leetcode 535: Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Solutions:

- [Approach 1: Basic HashMap Solution](#approach-1-basic-hashmap-solution)
- [Approach 2: Random Generator with HashMap](#approach-2-random-generator-with-hashmap)
- [Approach 3: Hashing the URL](#approach-3-hashing-the-url)

### Approach 1: Basic HashMap Solution

**Intuition:**

The simplest way to tackle the problem of encoding and decoding TinyURL is to use a HashMap (or a map in Go language). We can store the original URL as a value and generate a unique key for it. When encoding, we store the original URL with an auto-incremented index as a key in a map. For decoding, we can just retrieve the original URL using this key.

**Code:**

```go
package main

import (
	"strconv"
)

type Codec struct {
	store map[int]string
	count int
}

// Constructor initializes the data structure
func Constructor() Codec {
	return Codec{
		store: make(map[int]string),
		count: 0,
	}
}

// Encode generates a new encoded URL
func (this *Codec) Encode(longUrl string) string {
	this.count++ // Increment the counter for unique key
	this.store[this.count] = longUrl // Store the original URL
	return "http://tinyurl.com/" + strconv.Itoa(this.count)
}

// Decode retrieves the original URL from the encoded one
func (this *Codec) Decode(shortUrl string) string {
	id, _ := strconv.Atoi(shortUrl[len("http://tinyurl.com/"):])
	return this.store[id] // Return the original URL
}
```

**Time Complexity:**
- Encoding and Decoding take O(1) time as it's just a matter of storing and retrieving data from a map.

**Space Complexity:**
- O(n), where n is the number of calls to `encode` as we store each URL.

### Approach 2: Random Generator with HashMap

**Intuition:**

To avoid sequential keys which can be predictable, we can use random strings to represent the shortened URLs. This would involve generating random alphanumeric strings and using this as the key in a map to point to the original URL.

**Code:**

```go
package main

import (
	"math/rand"
	"strings"
	"time"
)

const baseURL = "http://tinyurl.com/"
const codeLength = 6

var chars = []rune("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789")

type Codec struct {
	store map[string]string
}

func Constructor() Codec {
	rand.Seed(time.Now().UnixNano())
	return Codec{
		store: make(map[string]string),
	}
}

// Generates a random string of fixed length
func randomString() string {
	var sb strings.Builder
	for i := 0; i < codeLength; i++ {
		sb.WriteRune(chars[rand.Intn(len(chars))])
	}
	return sb.String()
}

// Encode generates a new encoded URL
func (this *Codec) Encode(longUrl string) string {
	code := randomString()
	for this.store[code] != "" { // Ensure the generated code is unique
		code = randomString()
	}
	this.store[code] = longUrl
	return baseURL + code
}

// Decode retrieves the original URL from the encoded one
func (this *Codec) Decode(shortUrl string) string {
	code := shortUrl[len(baseURL):]
	return this.store[code]
}
```

**Time Complexity:**
- O(1) time for both encoding and decoding, assuming random string generation is efficient.

**Space Complexity:**
- O(n), where n is the number of URLs stored.

### Approach 3: Hashing the URL

**Intuition:**

In the case we want a deterministic approach rather than a random one incurring potential collisions, we can use a hashing function to generate a unique identifier for each URL. A simple hash or MD5 can help achieve this.

**Code:**

```go
package main

import (
	"crypto/md5"
	"encoding/hex"
	"strings"
)

const baseURL = "http://tinyurl.com/"

type Codec struct {
	store map[string]string
}

func Constructor() Codec {
	return Codec{
		store: make(map[string]string),
	}
}

// Generate MD5 hash
func generateHash(longUrl string) string {
	hasher := md5.New()
	hasher.Write([]byte(longUrl))
	return hex.EncodeToString(hasher.Sum(nil))[:6]
}

// Encode generates a new encoded URL
func (this *Codec) Encode(longUrl string) string {
	hash := generateHash(longUrl)
	this.store[hash] = longUrl
	return baseURL + hash
}

// Decode retrieves the original URL from the encoded one
func (this *Codec) Decode(shortUrl string) string {
	hash := shortUrl[len(baseURL):]
	return this.store[hash]
}
```

**Time Complexity:**
- Encoding takes O(1) time if hashing is assumed as O(1).
- Decoding takes O(1) time.

**Space Complexity:**
- O(n), where n is the number of unique URLs as each URL and its hash are stored.

