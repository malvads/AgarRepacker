# AgarRepacker

the goal of the repacker was to act as a proxy between agario server and old agario clients, Miniclip (Agar.IO founders) encrypted the whole client-server comunnication using an special algorithm to calculate a new key for every packet sent to
the server, tthey obfuscated this using LLVM to emscripten to execute on the browser, wich i had to reverse engineer

This project was created when I was a kid and later i decided to publish it on my old GitHub account, so please forgive the shitty code. The goal of AgarRepacker was to act as a proxy between the Agar.io server and old Agar.io clients. This allows you to play on the latest servers using legacy clients that don't natively support newer protocols.

Miniclip (the founders of Agar.io) encrypted the client-server communication with a special algorithm that generates a unique 32 bit key for each packet sent to the server.
```javascript
        key = Math.imul(key, 1540483477) | 0
        key = (Math.imul(key >>> 24 ^ key, 1540483477) | 0) ^ 114296087
        key = Math.imul(key >>> 13 ^ key, 1540483477) | 0
        key = key >>> 15 ^ key
        return key
```

Then a XOR operation is made for every byte of the "real" packet that is going to be sent to the server using the key, after sent, the key is rotated.

They obfuscated the full core code using LLVM and Emscripten to execute it in the browser. To make AgarRepacker work, I had to reverse engineer the encryption and communication protocol.
