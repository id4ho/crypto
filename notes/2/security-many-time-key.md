#### Security for many-time key

When you use a key more than once, the adversary can see CTs that were encrypted
with the same key.

For this exercise the Adversary will have "chosen plaintext attack" and be able
to see the CT for any message of his choosing. His goal is to break semantic
security. (this is a conservative modelling of real life)

If the secret key is to be used multiple times -> given the same plaintext
message twice, the system must produce different outputs.

**Two ways to do this**
- Randomized encryption: Each message maps to a set of possible plaintexts and
  the algorithm picks a random string that leads to one c in the set. This means
that the ciphertext must be longer than the message and the sets must be
disjoint for the decryption algo to recover the message.
- Nonce-based encryption: In addition to the normal inputs, k, m the encryption
  algo takes a nonce, n. This nonce is a value that must change from message to
message and can never repeat (the k,n pair must never be used more than once).
In https, the nonce is implied by the packet count (since this is shared between
client and server). Other than a counter, it can also be generated randomly.
This is useful since neither side needs to maintain state and works when
encryption needs to happen on multiple devices.

##### CBC (Cipher Block Chaining)

Pick initial IV (Initialization Vector), which must be the same length as one
block, and xor the first block of the message with it. Then encrypt that result
through the block cipher. The result (CT block) will be used to xor the second
block of plaintext before it is encrypted. The IV must be prepended to the CT in
order to aid decryption.

If the IV is predictable (i.e. the adversary knows what IV will be used for
m<sub>1</sub> after getting the CT for m<sub>0</sub> then CBC is NOT CPA secure.

**Nonce based CBC**
Cipher block chaining with unique nonce: key = k, k<sub>1</sub>, use
k<sub>1</sub> for encrypting the nonce, and k for the actual message. The output
of encrypting the unique nonce with k<sub>1</sub> is used as the IV for the
first block of the actual message in the CBC chain.

What happens if the last block is not a full block length? You simply pad it
with however many bytes you need. If 5 bytes are missing, you append 5 bytes
encoded as 5. The decryptor will look at the last bit of the message and throw
out the number of bytes that are equal to it. What is the last byte is exactly
the RIGHT length?? Will the decryptor still try to throw some out? Yes, so you
must pad the message with a dummy block. (16 bytes each byte with the number 16)


##### Randomized Counter Mode (CTR)

This mode is superior to CBC. It uses a PRF and doesn't need a block cipher
because we will never be inverting the function, F.

Here we choose a random IV for every message and use it as the message to
encrypt with k for the first block. Then we add 1 to IV and encrypt it with k
for the second block and so on and so forth until we've generated a random
sequence with as many blocks as the message and then we xor the original message
with it. (stream cipher style). The IV is prepended to the message so that the
decryptor can generate the pad and decrypt the message. Because this is xor
style and the block cipher part just generates the 'one time pad' then you don't
need to pad blocks, you can select less of the generated pad to use for xoring
the message!

NOTE: This is parallelizable unlike the CBC which is inherently sequential!


