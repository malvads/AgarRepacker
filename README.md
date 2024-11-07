# AgarRepacker

This project was created when I was a kid and later published on my old GitHub account, so please forgive the shitty code. 😅 AgarRepacker was intended to act as a proxy between Agar.io servers and legacy Agar.io clients, allowing you to use old clients that didn’t support newer server protocols. It was my attempt to bridge the gap between the old and new versions of Agar.io.

Miniclip (the creators of Agar.io) encrypted the entire client-server communication using a special algorithm to generate a new 32-bit key for each packet sent to the server. They obfuscated the core code using LLVM and Emscripten to run it in the browser. To make AgarRepacker work, I had to reverse-engineer the encryption algorithm and the communication protocol.

# Main Encryption/Decryption Breakdown
Miniclip used a special algorithm to generate a unique key for each packet. This key is then used for XOR encryption on each byte of the packet before sending it to the server. Here's the main key-generation algorithm:

```javascript
        key = Math.imul(key, 1540483477) | 0
        key = (Math.imul(key >>> 24 ^ key, 1540483477) | 0) ^ 114296087
        key = Math.imul(key >>> 13 ^ key, 1540483477) | 0
        key = key >>> 15 ^ key
        return key
```

After the key is generated, a XOR operation is applied to each byte of the packet using the key. The key is rotated after each packet is sent.

AgarRepacker was created to decode and re-encrypt the packets on the fly so old clients could still communicate with newer servers.
