#### CBC Padding Attacks

Two types of errors in TLS decryption:
1. Padding error - after decryption the padding at the end doesn't match
2. MAC error - after decryption the pad matches but the MAC doesn't

If the attacker can differentiate between the two, we have a "padding oracle",
which is chosen ciphertext attack. The attacker has learned something about the
plaintext just by submitting ciphertexts.

An attacker with a Padding Oracle can break encryption.

Older versions of TLS (1.0) gave a different message depending on which type of
error there was. Even after they returned the same error for both types, there
was a way to determine the error type using a timing attack.

Using a padding oracle you can guess the last byte of the plaintext message
(2^8=256 possibilities). You take a ciphertext, and suppose you want to decrypt
a specific block. You can submit that block and the previous block only, c[0]
and c[1]. Then you can substitute your guess, g, xord with 0x01 (which is a
valid padding block). Since the previous ciphertext is xored with the decrypted
block then if you guess correctly the last byte of the decryption of c[1] will
be xord with g xor 0x01. If your guess was correct that will cancel out the
decrypted message an the last bit of the decryption will be 0x01 which is a
valid pad, and therefore it will not be a Padding error.. only a MAC Error. Then
you can repeat the process for subsequent bytes.

TLS will actually drop a connection with a failed decryption and renegotiate a
different key making this attack generally infeasible. It is still feasible on a
IMAP server running on top of TLS. The IMAP mail server will query for new mail
every five minutes or so sending the same message "LOGIN 'username' 'password'"
which means the attack still works as the attacker can still make a guess every
few minutes, which will eventually recover the password over several hours.

So this TLS needs to always check MAC regardless of whether the padding is
valid or not.

So TLS should have used Encrypt-then-MAC: no issue since the MAC is checked
first, then pad is checked. MAC-then-CBC can provide authenticated encryption
but ONLY if you don't reveal why decryption failed.
