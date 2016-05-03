#### Compression Functions

One way to build a commpression function is with a block cipher (maps n bits to
n bits).

One example is the Davies-Meyer compression function which takes the message as
the key to the block cipher and the chaining variable (H<sub>i</sub>) and
xors the output with the same chaining variable to produce the next chaining
variable (or the hash if it's the last block)

This construction is as follows: h(H, m) = E(m, H) xor H

##### SHA 256 construction

- Merkle-Damgard function
- Davies-Meyer compression function
- Block-cipher: SHACAL-2

It processes the message 512 bits at a time and outputs a 256 bit hash
(chaining variable)

##### Provable compression functions

These use hard problems from number theory to construct secure compression
functions.

One example:
Chose a random 2000-bit prime, p, and two random numbers, u and v, between 1 and
p.

For m,h within {0, ... ,p-1}
define h(H,m) = u<sup>H</sup> \* v<sup>m</sup> (mod p)

Theorem: finding a collision for h(.,.) is as hard as solving the "discrete-log"
modulo p (which is a very hard problem in number theory)

One reason this isn't used in practice despite being provable secure is that it
is very **slow**.
