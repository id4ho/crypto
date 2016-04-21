### Attacks on Stream Ciphers and One Time Pads

#### Attack: Two Time Pad

Never use a OTP or stream cipher key more than once. If you do, an attacker can
mount a ciphertext only attack by simply xoring the ciphertexts together:

C<sub>1</sub> XOR C<sub>2</sub> -> M<sub>1</sub> XOR M<sub>2</sub>

There is enough redundancy in the English language (and ASCII encoding) that
having the two messages xor'd together allows an attacker to determine the two
messages.

Real world examples: Project Venona (interesting backstory), MS-PPTP (windows
NT), WEP

##### WEP
In the PRG the k is prepended with IV, a 24 bit number, that is incremented each
frame..i.e. to not use the same key more than once. Problem: IV was only 24
bits, which means the key is repeated after only 2^24 or 16 million frames. In
addition, on some 802.11 cards you can power cycle the router and cause the IV
to reset to 0.

If this isn't bad enough, the PRG used was RC4 and when you use many similar
keys with RC4 you can recover the key used with only around 1 million
ciphertexts (FMS 2001). Since 2001, better attacks have shown it only takes
around 40k.

#### Attack: no integrity (OTP is malleable)

An attacker can intercept and modify the ciphertext by xoring, p,
which will result in a decrypted message of m xor p. If an attacker knows the
structure of the message ("FROM EVE:...") they would be able to change the
ciphertext by xoring "FROM EVE:" xor "FROM BOB:" to the ciphertext and have the
decryption say that the message is from BOB instead of from EVE.
