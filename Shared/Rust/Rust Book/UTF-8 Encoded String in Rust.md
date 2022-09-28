# String in Rust
![Funny meme regarding Rust's strings](Attachments/Pasted%20image%2020220928144002.png)
*Funny meme in Rust's string data type*

String in Rust is implemented as a collection of bytes.

Rust has only one string type, which is the string slice type `str`, and it is often borrowed under the `&str` form. As mention in chapter 4, string slices are references to some UTF-8 encoded string data stored elsewhere in the computer's memory. String literals are stored in the program's binary; thus, it is a string slice.

The `String` type in Rust included in Standard Library, is a growable, mutable, owned UTF-8 encoded string type.

`String` type has many of the same operations as `Vec<T>` type as `String` is actually implemented as a vector of bytes with some restrictions, capabilities, and extra guarantees.

## String operations
To create a new string:
```rust
let mut s = String::new();
```

To create a string from literals:
```rust
let mut s1 = "hello".to_string();
// OR
let mut s2 = String::from("hello");
```

As strings are UTF-8 encoded, you can use whatever properly encoded data you want to stored as string:
```rust
let hello = String::from("السلام عليكم");
let hello = String::from("Dobrý den");
```

All of these are `String` values.

You can update the `String` by using the following operations:
```rust
let mut s = String::new();
s.push_str("hello"); // Append string literals
s.push('w'); // Append single character
```

Or using the *add* operator:
```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // s1 is invalid, as it has been moved to s3.
```

To concatenate multiple strings conveniently, consider using this approach:
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{}-{}-{}", s1, s2, s3);
```

To be noticed, Rust does not support indexing string by integer due to how Rust store strings under the hood. Let's see how Rust deal with strings in memory.

### String's Internal Presentation
Consider this declaration of string variable:
```rust
let s = String::from("hello");
```
Since string is a wrapper of `Vec<u8`> (vector of 8-bit bytes), the `s` string accounts for 4 bytes (1 byte/char)  in the system's memory.

But for this the below string, it takes up to the total of 24 bytes for encoding (not 12 bytes), because each Unicode scalar value in that string takes 2 bytes of storage. For that reason, indexing a single string's byte won't always correlate with valid Unicode scalar value.
```rust
let s = String::from("Здравствуйте");
```

### Bytes & Scalar values & Grapheme Clusters
There are 3 ways to look at string in Rust, as:
- Bytes
- Scalar values
- Grapheme clusters (*closest thing to letters*)

Considering this Hindi letter "नमस्ते":

- As representation of bytes, they look like this:
```rust
[224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164, 224, 165, 135]
```

- As representation of scalar values, they look like this:
```rust
['न', 'म', 'स', '्', 'त', 'े']
```
The `्` and `े` are diacritics (that don't make sense on their own) of the language.

- As grapheme cluster, they're actually 4 letters of what a person will interpret when looking at the word:
```rust
["न", "म", "स्", "ते"]
```

Due to that, Rust allows different ways of interpreting a raw string data so that each program can have their own way of interpreting the data, no matter what human language the data is in.

The ultimate reason of Rust not allowing indexing the String to get a character is that indexing is expected to be at a cost of *O(1)* time complexity. However, it isn't possible with Rust, to ensure the safety of the operation, Rust has to walk through the content of the strings to make sure there are enough valid characters for the performing index operation.

### Slicing Strings
For the above reason, indexing into the string is usually a bad idea because Rust does not know whether you want a byte, a character or a grapheme letter. Therefore, if you need a string type, you should use range `[]` syntax to get a string slice containing particular bytes:
```rust
let hello = "Здравствуйте";
let s = &hello[0..4];
```
As the `s` string will be a `&str` type containing the first 4 bytes of the character `[0..4]` and each of these characters accounts 2 bytes; therefore, we'll get the string slice `Зд`.

> NOTE: If you only get the first byte via [0..1], you will eventually end up in compile error as the `3` in the above string consists of 2 bytes in general.

### Methods of iterating string
If you want to iterate through individual Unicode scalar values, use `.chars()`:
```rust
for c in "Зд".chars() {
    println!("{}", c);
}
// It will print:
// З
// д
```

Or you can iterate through individual bytes, using `.bytes()`:
```rust
for b in "Зд".bytes() {
    println!("{}", b);
}
// It will print:
// 208
// 151
// 208
// 180
```

> NOTE: Always remember that a Unicode scalar value (char) can contains more than 1 byte.

Unluckily, getting grapheme clusters is not implemented in Rust's stdlib due to how complex they are in different language. You can refer to `crates.io` to find the package whose functionality serves you for personal purpose.

In general, String is definitely not a simple structure that most people have often thought about due to its diverse use cases. Rust's strict rules in handling UTF-8 type data is going to be really useful for the later development cycle when dealing with non-ASCII characters.

## References
- https://doc.rust-lang.org/stable/book/ch08-02-strings.html