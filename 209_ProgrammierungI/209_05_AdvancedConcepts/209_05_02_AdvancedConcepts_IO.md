

Character and byte streams

There are two kinds of streams:

- Byte streams use 8-bit blocks, i.e., they read and write bytes. This is useful for binary data.
    
- Character streams use 16-bit blocks, i.e., they read and write two bytes = one character*.
    
    They also translate internal Unicode-based character encoding to external encodings. Character streams are useful for text-based data.

---

All resources in Java are exposed as streams: a sequence of data elements made available over time.

Byte streams read/write 8-bit blocks, character streams read/write 16-bit blocks.  
Basic streams directly expose a resource.  
Higher-level streams read from/write to other streams.  
In practice, we usually have chains of streams – similar to the Chain of Responsibility pattern.

# 1 Basic streams



