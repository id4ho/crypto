#### Block Ciphers

This is a more robust primitive than a stream cipher ("crypto work horse")

There are two algorithms (E, D) that both take a key, k. These algorithms map
the PT block of n bits to a CT block of n bits.

##### Built by iteration

In a block cipher the key is expanded into a number of keys called round keys.
Then the round function, R(k, m), takes the current state of the input
(m<sub>i</sub>) and produces the next round message.

For different ciphers there are different numbers of rounds:
- 3DES -> 48 rounds
- AES-128 -> 10 rounds

##### PRF Psuedo Random Function
Defined over (K,X,Y) where F(K,X) -> Y. Exists an efficient algorithm to
evaluate F(K,X)

##### PRP Pseudo Random Permutation
This is the same as a block cipher.
Defined over (K,X) where F(K, X) -> X. Such that:
1. Exists an efficient algorithm to evaluate E(k, x)
2. Function E(k, m) is one-to-one
3. Exists an efficient inversion algorithm D(k, y)

A PRP is a PRF where X=Y and it's efficiently invertible.

##### What does it mean for a PRF to be secure
If Funs(X, Y) is the set of functions that map X to Y (with any key) and
S<sub>F</sub> is the set of functions that map X to Y when the key is chosen
from the valid pool, K, then a PRF is secure if a random function in Funs[X,Y]
is indistinguishable from a random function in S<sub>F</sub>.

##### An easy application for PRFs
Making a PRG
Let F: K * {0,1}<sup>n</sup> -> {0,1}<sup>n</sup> be a secure PRF.
Then the following G: K -> {0,1}<sup>nt</sup> is a secure PRG.
G(k) = F(k,0) || F(k,1) || F(k,2) || ... || F(k,t)

Secure from PRG property: F(k,\_) industinguishable from f(\_)
Key property: parallelizable

### Data Encryption Standard (DES)

Core idea is the Feistel Network:
Clever idea for building a block cipher out of arbitrary functions
f<sub>1</sub>,...,f<sub>d</sub>: {0,1}<sup>n</sup> -> {0,1}<sup>n</sup>

Input is defined as 2n and the 2n bits are split into two halves, L<sub>0</sub>
and R<sub>0</sub>. At each iteration, you apply the function to R and xor L with
the output to get the new R. The new L is simply the previous R. There are d
iterations.

R<sub>i</sub> = f<sub>i</sub>(R<sub>i-1</sub>) xor L<sub>i-1</sub>
L<sub>i</sub> = R<sub>i-1</sub>

For any functions this can be efficiently inverted. You never actually need
f<sup>-1</sup> because of the fact that half of the input become half of the
output each time. To work backwards from i+1 to get i:
R<sub>i</sub> = L<sub>i+1</sub> (per the bottom equation above)
L<sub>i</sub> = f<sub>i+1</sub>(L<sub>i+1</sub>) xor R<sub>i+1</sub>

this is a very general method for building invertible functions: key idea is
that the thing you applied the function to is still available after the
application. i.e. going either direction you can apply the function to the same
thing since it's available at both ends of the pipe.

*This is used in many Block ciphers but not AES*

A 3 round Fiestel network can turn a secure PRF into a secure PRP (PRP can be
inverted). (applying the same PRF each round but with a different key each time)

**DES** is a 16-round Fiestel network. It takes a 56 bit key that is expanded
into 16 48 bit keys, one for each round.

A single round function takes 32 bits (half the input), expands it to 48 bits,
and xors it with the round key (k<sub>i</sub>). This result is broken up into 8
sets of 6 bits. Each set goes into an "s-box" which takes the 6 bits and maps it
to 4 bits. These are concatted into 32 bits and that is the output for the
round.

S-box - simply a lookup table.

The s-boxes cannot be linear functions. If they are then the entirety of DES
would be linear and therefore the ciphertext could be defined as a matrix B (64
x 832) that you could multiply by a very long vector of the message and the 16
round keys, a (832 x 1). If this was the case you could take three outputs from
DES and xor them together to get:
DES(k, m<sub>1</sub>) xor DES(k, m<sub>2</sub>) xor DES(k, m<sub>3</sub>)
which would = DES(k, m<sub>1</sub> xor m<sub>2</sub> xor m<sub>3</sub>)

This would be a very easy test to find out that DES wasn't random.

In fact, even if you chose the s-boxes at random, it would result in an insecure
cipher (because it's close enough to linear)

The makers of DES specified strict rules for the choice of S boxes:
- no output bit should be close to a linear func. of the input bits
- S-boxes are 4-to-1 maps (every output has exactly 4 preimages)
- .. etc.
