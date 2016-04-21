### Symmetric Ciphers

If Alice is sending a message to Bob (she always is..) then Alice has an
Encryption algorithm `E` and Bob has a decryption algorithm `D`. In a symmetric
cipher, they both have access to the same key, `k`.

the message, `m`, undergoes the following:

`m` -> **E** -> `C` (`:= E(k, m)`) -> **D** -> `m`

#### Historic Examples:
1. Substitution Cipher: (fixed set of translations for alphabet)
2. Caesar Cipher: no key, shift by fixed number.

One way to break the substitution cipher is using letter frequencies. If the
message is in English, you can assume that the most frequent letter is 'e',
since the most frequent letters in English are:
1. 'e' -> 12.7%
2. 't' -> 9.1%
3. 'a' -> 8.1%

The next most common letters all have similar frequencies, at that point you
can switch to digrams (pairs of letters): 'he', 'an', 'in', 'th'

3. Vigener cipher: here you chose a word or phrase as a key, then 'xor' the
   message with the word or phrase repeated over and over:
k = CRYPTOCRYPTOCRYPT
m = WHATANICEDAYTODAY
c = ZZZJUCLUDTUNWGCQS

To break this cipher you look at groups of letters and analyze the letters in
the same position (in this example ZLW). You know that they were all encrypted
with the first letter of the key word. With this set you can check frequencies
again and the most frequent was likely the encryption of the letter 'e' and you
can then work backwards to get the key letter at that position. Then solve the
remaining places in a similar manner.

This is a ciphertext only attack.

4. Rotor machines (1870-1943)
One early example is the Hebern machine (single rotor)

This machine had a rotor with a key and the key would rotate one place every
time a new letter of the message was input. This means if you typed 3 c's in a
row, it would be encrypted by a different letter each time. 

Soon after this started to be used, it was broken with a ciphertext only attack
using only letter frequencies and digram/trigram frequencies.

Most famous was the Enigma, cracked using a ciphertext only attack by the
cryptographers (cryptanalysts) at Bletchley Park. The enigma had 3-5 rotors,
which could be set at any position (keyspace of 3-5^26)

5. DES (Digital Encryption Standard)
This was requested from the industry by the government. Keyspace is only 2^56
and should not be used today.

