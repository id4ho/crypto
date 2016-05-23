#### Attacking Non-Atomic Decryption

**SSH:**
Binary Packet Protocol:
seq. # || packet length || pad length || payload || pad || MAC tag
only the packet len. pad len. payload and pad are actually encrypted. This is
encrypt-then-mac.

**Decryption:**
1. decrypt packet length field only
2. read as many packets as length specifies
3. decrypt remaining ciphertext blocks
4. check MAC

The problem is that this is non-atomic. It doesn't take a block of input and
respond with a whole block of plaintext as output. It accepts byte by byte which
allows an attacker to feed a ciphertext block as the length block and then send
trash bytes until it gets a MAC error. The number of bytes sent when the error
is received is what the first 32 bits of the ciphertext decrypts to.

**Lesson: Don't use any decrypted fields before the decryption is verified!**

Additional Reading:
1. https://www.iacr.org/archive/crypto2001/21390309.pdf (21 pages)
2. https://www.cs.columbia.edu/~smb/papers/badesp.pdf (10 pages)
