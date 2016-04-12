#### RC4

Used in HTTPS, WEP etc.

Weaknesses of RC4:
1. The second byte of output is biased towards 0. Pr[byte<sub>2</sub> = "0"] =
   2/256 (i.e. twice as likely as it should be)
2. Probability of getting ("0", "0") is 1/256^2 + 1/256^3 (second part is the
   bias)
3. Related key attacks (discussed in prior lesson with WEP)

#### CSS (used to encrypt DVD movies)

Content Scrambling System

Designed to be a hardware stream cipher that is easy to implement. Based on a
Linear feedback shift register (LFSR) which involves 8 bit register. Every other
bit is combined via xor and shifted onto the front of the 8 bits (dropping the
last bit) each round.

DVD encryption (CSS): 2 LFSRs
GSM encryption: (A5/1,2): 3 LFSRs
Bluetooth (E0): 4 LFSRs

ALL BROKEN

#### Modern stream ciphers

Developed through the eStream project (concluded in 2008)

PRG: {0,1}<sup>s</sup> x R -> {0,1}<sup>n</sup>

where R is the nonce (a non-repeating value for a given key)

You can reuse the key because the pair (k,r) is what must be unique.

##### Salsa20

Designed from eStream for both HW and SW. One of five stream ciphers that came
out of the eStream project.

Involves 128 or 256 bit key and a 64 bit nonce. The key, nonce, counter are
expanded into 64 bytes (k is repeated, four 4-byte inputs are provided by the
spec) and a function, h, is applied to the 64 bytes ten times and the last
result is added (word by word) to the initial 64, i is incremented and you can
get another 64 bytes.
