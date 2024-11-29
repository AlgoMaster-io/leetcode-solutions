# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
go
```go
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
package main

import (
	"strconv"
)

type Codec struct {
	mapID map[int]string
	id    int
}

func Constructor() Codec {
	return Codec{mapID: make(map[int]string), id: 0}
}

// Encodes a URL to a shortened URL.
func (this *Codec) encode(longUrl string) string {
	this.id++
	this.mapID[this.id] = longUrl
	return "http://tinyurl.com/" + strconv.Itoa(this.id)
}

// Decodes a shortened URL to its original URL.
func (this *Codec) decode(shortUrl string) string {
	key, _ := strconv.Atoi(shortUrl[len("http://tinyurl.com/"):])
	return this.mapID[key]
}
```

## Approach 2: HashMap with Randomized Key

### Solution
go
```go
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
package main

import (
	"math/rand"
	"time"
)

type Codec struct {
	shortToLong map[string]string
	longToShort map[string]string
	characters  string
}

func Constructor2() Codec {
	return Codec{
		shortToLong: make(map[string]string),
		longToShort: make(map[string]string),
		characters:  "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789",
	}
}

func (this *Codec) getRandomKey() string {
	rand.Seed(time.Now().UnixNano())
	key := make([]byte, 6)
	for i := 0; i < 6; i++ {
		index := rand.Intn(len(this.characters))
		key[i] = this.characters[index]
	}
	return string(key)
}

// Encodes a URL to a shortened URL.
func (this *Codec) encode(longUrl string) string {
	if short, exists := this.longToShort[longUrl]; exists {
		return "http://tinyurl.com/" + short
	}

	var key string
	for {
		key = this.getRandomKey()
		if _, exists := this.shortToLong[key]; !exists {
			break
		}
	}
	this.shortToLong[key] = longUrl
	this.longToShort[longUrl] = key
	return "http://tinyurl.com/" + key
}

// Decodes a shortened URL to its original URL.
func (this *Codec) decode(shortUrl string) string {
	key := shortUrl[len("http://tinyurl.com/"):]
	return this.shortToLong[key]
}
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
go
```go
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
package main

import (
	"bytes"
)

type Codec struct {
	mapID     map[string]string
	characters string
	base       int
	id         int
}

func Constructor3() Codec {
	return Codec{
		mapID:      make(map[string]string),
		characters: "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789",
		base:       len("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"),
		id:         0,
	}
}

func (this *Codec) toBase62(id int) string {
	var encoded bytes.Buffer
	for id > 0 {
		encoded.WriteByte(this.characters[id%this.base])
		id /= this.base
	}
	runes := []rune(encoded.String())
	for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
		runes[i], runes[j] = runes[j], runes[i]
	}
	return string(runes)
}

func (this *Codec) decodeBase62(base62 string) int {
	id := 0
	for _, c := range base62 {
		id = id*this.base + bytes.IndexByte([]byte(this.characters), byte(c))
	}
	return id
}

// Encodes a URL to a shortened URL.
func (this *Codec) encode(longUrl string) string {
	this.id++
	shortKey := this.toBase62(this.id)
	this.mapID[shortKey] = longUrl
	return "http://tinyurl.com/" + shortKey
}

// Decodes a shortened URL to its original URL.
func (this *Codec) decode(shortUrl string) string {
	key := shortUrl[len("http://tinyurl.com/"):]
	return this.mapID[key]
}
```

