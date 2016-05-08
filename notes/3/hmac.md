#### HMAC

Standard way to convert collision resistant hash function into a MAC

Use SHA256 as the hashing function (output is 256 bits, input is 512)

S(k,m) = H(k xor opad, H(k xor ipad || m))

k xor ipad is first block fed into the compression function (along with the IV).
Then the output and the first message block. Eventually when all the message has
been fed through, you feed that into another MD function that took as input k
xor opad and IV. The output of that first block in the second chain and the
output from the hashed message feed into the second compression function in
chain 2 and the output is the MAC.

This can be proven secure under certain PRF assumptions about h(.,.), with
security bounds similar to NMAC

In TLS: must support HMAC-SHA1-96 (SHA1 outputs 160 bits and this truncates to
96 bits). SHA1 is no longer considered a secure hash function..how can we use it
here? It doesn't need to be collision resistant, it only needs to act as a PRF
when either input is allowed to be the key which as we know is still correct.

##### Timing attacks on MAC Verification

Typically these verification systems compute the MAC and then perform a byte by
byte comparison of the given tag and the computed MAC. The first time it finds
an inequality it terminates with a false result.
This introduces a significant timing attack on a library coded like this.
The attacker will submit a message, tag pair to the server where the tag is just
random. He'll measure the response time and submit another random tag. He then
sees that the avg response time for an evaluation of a tag with an incorrect
first byte. You then start with 0 and work your way through all possible bytes
until it take a bit longer and you know the submitted tag's first byte is
correct, repeat for subsequent bytes.

What you should do is zip the tag with the computed tag, and iterate over all
the pairs. result = 0 should be set outside the loop and then or the xor of the
bytes onto the result. i.e. result |= ord(x) ^ ord(y)

Even this can be tough with compiler optimizations.

Another way to avoid a timing attack:
mac = HMAC(key, msg)
return HMAC(key, mac) == HMAC(key, provided_sig)
Here the adversary doesn't know what two strings are being compared and
therefore cannot mount a timing attack.
