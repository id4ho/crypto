#### Message Authentication Code (MACs)

One example of MAC is a two algorithm integrity system, I = (S,V) where S(k, m)
outputs a tag, t, in T and V(k, m, t) outputs 'y' or 'n'.

Consistency requirement: for any key, k, in K and any message, m, in M it must
hold that V(k, m, S(k, m)) = 'y'

Integrity **requires** a shared key between alice and bob.

The classic checksum algorithm, cyclic redundancy check or CRC, is only meant to
detect random errors, not malicious ones.

To evaluate the security of MACs we use the following paradigm:

- **attack**: chosen plaintext attack
- **goal**: existential forgery (produce some new valid message/tag pair) or
  find another tag, t', that works for one of the m,t pairs.

##### Protecting system files on disk

Suppose at install the computer asks the user for a password. It then takes this
password and adds a tag to every system file.

If later a virus infects the system and modifies system files, the user can find
out which system files were changed by logging into a clean OS providing his
password and checking all the tags.

\* caveat: the virus could swap files in this case without detection (since the
m,t would still be a valid pairs for each. So the OS would need to include the
file name/path in the message used for tag generation.

##### How do we build secure MACs

A secure PRF yields a secure MAC:
If the output of the PRF is large (say 2^80) then you are guaranteed a secure MAC. (short
output would allow the adversary to randomly guess the tag and be right a
non-negligible percentage of times)

The advantage of the adversary against the MAC will be less than or equal to the
advantage against the PRF (which is negligible) plus the rate at which the
adversary could guess correctly 1/|Y| (where |Y| is 2^length)

##### The McDonalds problem

We have AES which is a secure PRF for 16 byte blocks and we have proved that it
would be secure as a MAC.

Given this little MAC how can we build a big MAC out of it?

##### Encrypted CBC-MAC (AES-based 802.11.i)

Let F: K, X -> X be a PRP
Define a new PRF F<sub>ECBC</sub>: K<sup>2</sup>, X<sup><=L</sup> -> X

This is essentially CBC but instead of spitting out the encrypted block **and**
using it as an input IV to xor the next block, you **only** use it as an IV for
the next block. Something similar could probably be done where you just use the
CBC as normal and xor every block on top of each other to get a smaller length
tag. The last output block, must be encrypted again with k<sub>1</sub> (hence
the K<sup>2</sup>) which will then serve as the tag. (if you skip the step you
the algo is called 'raw CBC' which is actually insecure.

Raw CBC -> generate an existential forgery with 1 chosen message attack: request
the tag for a one block `m`. We can then append `m` xor `t` to the original
message and the tag generated would be the same. So that pair is a valid
forgery:
rawCBC(k, (m, m xor t))
=> F(k, F(k, m) xor (m xor t))
=> F(k, t xor m xor t)
=> F(k, m)
=> `t`

##### NMAC (basis of HMAC)

Let F: K, X -> K be a PRF
Define a new PRF F<sub>NMAC</sub>: K<sup>2</sup>, X<sup><=L</sup> -> K

The output from running block m<sub>i</sub> through the new PRF F<sub>NMAC</sub>
is used as the key for the next m<sub>i+1</sub> block.

if you stop here, you have a **cascade** function.

The last step in NMAC is to pad the last output so that it's in the set X and
run the function again with that as input and the second key. This last step is
necessary for security.

For this example: I can create an existential forgery if the last step is
skipped. I ask for the encryption of m and get back the tag then I can append
another block to the message to create m' and using the tag as the key input,
run the function F<sub>NMAC</sub> again to get the new tag for m'.

##### Security parameters

Both big MAC functions have a limit to how many messages you can send with the
same key: |X|<sup>1/2</sup> for ECBC MAC and |K|<sup>1/2</sup> for NMAC.

These numbers are derived from the fact that they both have the extension
property (defined below) and the birthday paradox making a collision likely
after that many messages have been MAC'd

They also both have the extension property. This means if you find a collision
at x and y then there will also be a collision at x||word and y||word since
after the collision there the same calculations get run with same inputs.

There is an attack on these functions due to this property and the Birthday
Paradox:

##### An attack

1. Issue |Y|<sup>1/2</sup> message queries for rand messages in X obtain
(m<sub>i</sub>, t<sub>i</sub>) for all queries.

2. Find a collision t<sub>u</sub> and t<sub>v</sub> for u!=v (one exists with high
probability due to b-day paradox)

3. Choose some w and query for t:= F<sub>big_mac</sub>(k, m<sub>u</sub>||w)

4. Output a forgery (m<sub>v</sub>||word, t)

##### MAC Padding

Padding function must be invertible and one-to-one. If it is not, it provides
another avenue for existential forgery. (i.e. padding with zeros is a horrible
idea)

ISO recommendation is to pad with 10...0. Corner case: original message length
divides evenly into blocks, add a full dummy padding block.

##### CMAC (NIST standard)

This is a variant of CBCMAC that takes 3 keys (k<sub>1</sub> and k<sub>2</sub>
are often derived from k) and does not require full block padding in the event
of a message that evenly divides into block length. If the message needs
padding, you pad using ISO recommendation and then xor with k<sub>1</sub> prior
to last function application. If it doesn't need padding you xor with
k<sub>2</sub> prior to last function application.

The result is no need to use a full dummy pad ever and this also thwarts the
extension attack.

##### PMAC (parallel MAC)

This is just a parallel MAC computation. Break message into blocks, xor with
function P(k, 0), apply the PRF and then xor all the outputs together. You must
include this P(k, n) function because otherwise the blocks of the message could
be shuffled and there would be no difference in the tag computed leading to an
easy forgery.

One property here is that you can also recompute the tag of a slightly altered
message without doing the entire computation. Each message block can be swapped
out. Decrypt the tag, then xor the original message block into the xor blob
again to remove it, add in the new block.

##### One time MAC

Can be faster than PRF-based MACS and secure against **all** adversaries

Let q be a large prime (e.g. q = 2^128 + 51)

key = (k, a) which are two random ints in [1, q]
msg = break message into 128 bit blocks and treat them as ints

S(key, msg) = P<sub>msg</sub>(k) + a (mod q)

Where P<sub>msg</sub>(x) = m[L]\*x<sup>L</sup> + ... + m[1]x is a polynomial of
degree L

Fact: given S(key, msg<sub>1</sub>) adversary has no info about S(key,
msg<sub>2</sub>)

##### Carter-Wegman MAC

Many time MAC (from OT MAC)

Let (S,V) be a secure OT MAC over (K<sub>l</sub>, M, {0,1}<sup>n</sup>)
Let F: K<sub>F</sub> x {0,1}<sup>n</sup> -> {0,1}<sup>n</sup> be a secure PRF.

CW((k1, k2), m) = (r, F(k1,r) xor S(k2,m)) where r is a random nonce in
{0,1}<sup>n</sup>.

This is nice because a really large message can be computed quickly reusing the
really fast S function.

Output is {0,1}<sup>2n</sup> though because you must include the nonce used.

##### Further Reading

There are some papers on MACs recommended at the end of the PMAC and
Carter-Wegman MAC video
